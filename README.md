<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Spreadsheet Game with Charts</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            padding: 20px;
        }

        table {
            width: 100%;
            border-collapse: collapse;
        }

        table, th, td {
            border: 1px solid #000;
        }

        th, td {
            text-align: left;
            padding: 8px;
            width: 4cm;
            height: 1cm;
        }

        #chartContainer {
            margin-top: 20px;
        }

        button {
            margin: 10px 5px;
            padding: 10px;
            font-size: 16px;
        }

        .error {
            color: red;
            font-size: 14px;
            margin-top: 10px;
        }
    </style>
</head>
<body>

    <h1>Spreadsheet Game: Student Marks</h1>

    <!-- Table for entering student names and marks -->
    <table id="spreadsheet">
        <tr>
            <th>A</th>
            <th>B</th>
        </tr>
        <!-- Empty rows to allow user input -->
        <tr><td contenteditable="true"></td><td contenteditable="true"></td></tr>
        <tr><td contenteditable="true"></td><td contenteditable="true"></td></tr>
        <tr><td contenteditable="true"></td><td contenteditable="true"></td></tr>
        <tr><td contenteditable="true"></td><td contenteditable="true"></td></tr>
        <tr><td contenteditable="true"></td><td contenteditable="true"></td></tr>
    </table>

    <!-- Error message container -->
    <div id="errorMessage" class="error"></div>

    <!-- Buttons for generating charts -->
    <div id="chartControls">
        <button onclick="generateChart('bar')">Bar Chart</button>
        <button onclick="generateChart('pie')">Pie Chart</button>
        <button onclick="generateChart('line')">Line Chart</button>
    </div>

    <!-- Chart container -->
    <div id="chartContainer">
        <canvas id="chartCanvas" width="400" height="300"></canvas>
    </div>

    <!-- Chart.js Library -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

    <!-- JavaScript Logic -->
    <script>
        // Function to gather data from the table
        function getTableData() {
            let names = [];
            let marks = [];
            const table = document.getElementById("spreadsheet");
            const errorMessage = document.getElementById("errorMessage");

            // Clear previous error message
            errorMessage.textContent = '';

            for (let i = 1; i < table.rows.length; i++) { // Start from 1 to skip header
                let name = table.rows[i].cells[0].innerText.trim();
                let mark = parseInt(table.rows[i].cells[1].innerText.trim());

                // Validate that names are not empty and marks are between 10 and 20
                if (!name) {
                    errorMessage.textContent = 'Please enter student names.';
                    return null;
                }
                if (isNaN(mark) || mark < 10 || mark > 20) {
                    errorMessage.textContent = 'Please enter marks between 10 and 20 for all students.';
                    return null;
                }

                names.push(name);
                marks.push(mark);
            }

            return { names, marks };
        }

        // Function to generate chart
        function generateChart(type) {
            const data = getTableData();
            const errorMessage = document.getElementById("errorMessage");

            // If there's invalid data, don't proceed to chart generation
            if (!data) {
                return;
            }

            const ctx = document.getElementById("chartCanvas").getContext("2d");

            // Destroy the existing chart before creating a new one
            if (window.chart) {
                window.chart.destroy();
            }

            // Random color generator for charts
            const colors = data.names.map(() => {
                return 'rgba(' + Math.floor(Math.random() * 256) + ',' +
                                Math.floor(Math.random() * 256) + ',' +
                                Math.floor(Math.random() * 256) + ', 0.6)';
            });

            // Create the chart
            window.chart = new Chart(ctx, {
                type: type,
                data: {
                    labels: data.names,
                    datasets: [{
                        label: 'Student Marks',
                        data: data.marks,
                        backgroundColor: colors,
                        borderColor: colors.map(color => color.replace('0.6', '1')),
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    scales: {
                        y: {
                            beginAtZero: true,
                            max: 20 // Limit max to 20 as per instructions
                        }
                    }
                }
            });
        }
    </script>
</body>
</html>
