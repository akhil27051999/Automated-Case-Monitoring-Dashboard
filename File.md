// ==UserScript==
// @name         Case Monitoring - NA
// @namespace    http://tampermonkey.net/
// @version      1.7
// @description  Monitor case statuses and highlight unassigned cases based on time thresholds
// @author       thakhilk@
// @match        https://paragon-na.amazon.com/hz/dox-search?searchQuery*
// @grant        GM_addStyle
// ==/UserScript==

(function () {
    'use strict';

    let dashboard = null;

    function createDashboard() {
        dashboard = document.createElement('div');
        dashboard.style.position = 'fixed';
        dashboard.style.top = '20px';
        dashboard.style.right = '10px';
        dashboard.style.backgroundColor = '#F9F9F9';
        dashboard.style.color = '#EA6A47';
        dashboard.style.padding = '8px';
        dashboard.style.borderRadius = '8px';
        dashboard.style.boxShadow = '0 4px 8px rgba(0, 0, 0, 0.2)';
        dashboard.style.fontSize = '12px';
        dashboard.style.zIndex = '1000';
        dashboard.style.textAlign = 'left';
        dashboard.style.maxWidth = '320px';
        dashboard.style.overflowY = 'auto';
        dashboard.style.maxHeight = '50vh';

        dashboard.innerHTML = `
            <h3>Case Monitoring Dashboard - NA</h3>

            <div style="margin-bottom: 5px; color: black; background-color: #dadada; padding: 5px;">
                <strong>Reopened: </strong><span id="reopenedCount">0</span>
                <button id="btnReopened">Show IDs</button>
                <div id="ReopenedIds" style="display: none; margin-top: 5px;"></div>
            </div>

            <div style="margin-bottom: 5px; color: black; background-color: #e0f1f7; padding: 5px;">
                <strong>Unassigned: </strong><span id="unassignedCount">0</span>
                <button id="btnUnassigned">Show IDs</button>
                <div id="UnassignedIds" style="display: none; margin-top: 5px;"></div>
            </div>

            <div style="margin-bottom: 5px; color: black; background-color: #dcedc8; padding: 5px;">
                <strong>Work-in-Progress: </strong><span id="workInProgressCount">0</span>
                <button id="btnWork-in-Progress">Show IDs</button>
                <div id="Work-in-ProgressIds" style="display: none; margin-top: 5px;"></div>
            </div>

            <div style="margin-bottom: 5px; color: black; background-color: #fff9c4; padding: 5px;">
                <strong>Unassigned 20-24 mins: </strong><span id="unassigned20To24">0</span>
                <button id="btnUnassigned20To24">Show IDs</button>
                <div id="unassigned20To24Ids" style="display: none; margin-top: 5px;"></div>
            </div>

            <div style="margin-bottom: 5px; color: black; background-color: #ffae90; padding: 5px;">
                <strong>Unassigned 25-29 mins: </strong><span id="unassigned25To29">0</span>
                <button id="btnUnassigned25To29">Show IDs</button>
                <div id="unassigned25To29Ids" style="display: none; margin-top: 5px;"></div>
            </div>

            <div style="margin-bottom: 5px; color: black; background-color: #ed6542; padding: 5px;">
                <strong>Unassigned >= 30 mins: </strong><span id="unassignedOver30">0</span>
                <button id="btnUnassignedOver30">Show IDs</button>
                <div id="unassignedOver30Ids" style="display: none; margin-top: 5px;"></div>
            </div>
        `;
        document.body.appendChild(dashboard);

        // Add event listeners for buttons
        document.getElementById('btnReopened').addEventListener('click', () => toggleVisibility('ReopenedIds'));
        document.getElementById('btnUnassigned').addEventListener('click', () => toggleVisibility('UnassignedIds'));
        document.getElementById('btnWork-in-Progress').addEventListener('click', () => toggleVisibility('Work-in-ProgressIds'));
        document.getElementById('btnUnassigned20To24').addEventListener('click', () => toggleVisibility('unassigned20To24Ids'));
        document.getElementById('btnUnassigned25To29').addEventListener('click', () => toggleVisibility('unassigned25To29Ids'));
        document.getElementById('btnUnassignedOver30').addEventListener('click', () => toggleVisibility('unassignedOver30Ids'));
    }

    function toggleVisibility(elementId) {
        const element = document.getElementById(elementId);
        if (element.style.display === 'none') {
            element.style.display = 'block';
        } else {
            element.style.display = 'none';
        }
    }

    function updateDashboard(
        reopenedCount,
        unassignedCount,
        workInProgressCount,
        unassigned20To24,
        unassigned25To29,
        unassignedOver30,
        reopenedIds,
        unassignedIds,
        workInProgressIds,
        unassigned20To24Ids,
        unassigned25To29Ids,
        unassignedOver30Ids
    ) {
        document.getElementById('reopenedCount').textContent = reopenedCount;
        document.getElementById('unassignedCount').textContent = unassignedCount;
        document.getElementById('workInProgressCount').textContent = workInProgressCount;
        document.getElementById('unassigned20To24').textContent = unassigned20To24;
        document.getElementById('unassigned25To29').textContent = unassigned25To29;
        document.getElementById('unassignedOver30').textContent = unassignedOver30;

        document.getElementById('ReopenedIds').innerHTML = reopenedIds.map(id => createHyperlink(id, 'yellow')).join('<br>');
        document.getElementById('UnassignedIds').innerHTML = unassignedIds.map(id => createHyperlink(id, 'lightblue')).join('<br>');
        document.getElementById('Work-in-ProgressIds').innerHTML = workInProgressIds.map(id => createHyperlink(id, 'green')).join('<br>');
        document.getElementById('unassigned20To24Ids').innerHTML = unassigned20To24Ids.map(id => createHyperlink(id, 'orange')).join('<br>');
        document.getElementById('unassigned25To29Ids').innerHTML = unassigned25To29Ids.map(id => createHyperlink(id, 'orange')).join('<br>');
        document.getElementById('unassignedOver30Ids').innerHTML = unassignedOver30Ids.map(id => createHyperlink(id, 'red')).join('<br>');
    }

    function createHyperlink(caseId, blue) {
        const baseUrl = 'https://paragon-na.amazon.com/hz/view-case?caseId=';
        return `<a href="${baseUrl}${caseId}" target="_blank" style="blue: ${blue}; text-decoration: none;">${caseId}</a>`;
    }

    function countCasesByStatus() {
        try {
            let reopenedCount = 0;
            let unassignedCount = 0;
            let workInProgressCount = 0;
            let unassigned20To24 = 0;
            let unassigned25To29 = 0;
            let unassignedOver30 = 0;
            let reopenedIds = [];
            let unassignedIds = [];
            let workInProgressIds = [];
            let unassigned20To24Ids = [];
            let unassigned25To29Ids = [];
            let unassignedOver30Ids = [];

            const cases = document.querySelectorAll('table tbody tr');

            cases.forEach((row) => {
                const statusCell = row.querySelector('td:nth-child(8)');
                const timestampCell = row.querySelector('td:nth-child(9)');
                const idCell = row.querySelector('td:nth-child(2) a');

                if (statusCell && timestampCell && idCell) {
                    const status = statusCell.textContent.trim();
                    const timestampText = timestampCell.textContent.trim();
                    const timestamp = parseTimestamp(timestampText);
                    const currentTime = new Date();
                    const timeLapsed = (currentTime - timestamp) / (60 * 1000); // Convert to minutes

                    if (status === 'Reopened') {
                        reopenedCount++;
                        reopenedIds.push(idCell.textContent.trim());
                    } else if (status === 'Unassigned') {
                        unassignedCount++;
                        unassignedIds.push(idCell.textContent.trim());

                        if (timeLapsed > 20 && timeLapsed <= 24) {
                            unassigned20To24++;
                            unassigned20To24Ids.push(idCell.textContent.trim());
                        } else if (timeLapsed > 24 && timeLapsed <= 29) {
                            unassigned25To29++;
                            unassigned25To29Ids.push(idCell.textContent.trim());
                        } else if (timeLapsed >= 30) {
                            unassignedOver30++;
                            unassignedOver30Ids.push(idCell.textContent.trim());
                        }
                    } else if (status === 'Work-in-Progress') {
                        workInProgressCount++;
                        workInProgressIds.push(idCell.textContent.trim());
                    }
                }
            });

            updateDashboard(
                reopenedCount,
                unassignedCount,
                workInProgressCount,
                unassigned20To24,
                unassigned25To29,
                unassignedOver30,
                reopenedIds,
                unassignedIds,
                workInProgressIds,
                unassigned20To24Ids,
                unassigned25To29Ids,
                unassignedOver30Ids
            );
        } catch (err) {
            console.error('Error counting cases:', err);
        }
    }

    function parseTimestamp(timestampText) {
        const parsedDate = Date.parse(timestampText);
        if (!isNaN(parsedDate)) {
            return new Date(parsedDate);
        }
        console.warn('Unable to parse timestamp:', timestampText);
        return new Date(); // Fallback to current time to avoid crashes
    }

    function waitForTableAndInitialize() {
        const tableObserver = new MutationObserver(() => {
            const table = document.querySelector('table tbody');
            if (table) {
                tableObserver.disconnect();
                createDashboard();
                countCasesByStatus();

                const observer = new MutationObserver(() => {
                    countCasesByStatus();
                });

                observer.observe(table, {
                    childList: true,
                    subtree: true,
                });
            }
        });

        tableObserver.observe(document.body, {
            childList: true,
            subtree: true,
        });
    }

    window.addEventListener('load', () => {
        waitForTableAndInitialize();
    });

    GM_addStyle(`
        body {
            font-family: Arial, sans-serif;
        }
        button {
            margin-top: 3px;
            padding: 3px;
            font-size: 12px;
            background-color: #0560e8;
            color: white;
            border: none;
            border-radius: 2px;
            cursor: pointer;
        }
        button:hover {
            background-color: #007BFF;
        }
    `);
})();
