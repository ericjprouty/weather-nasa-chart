<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Weather Since 1880 by NASA</title>
    <link rel="stylesheet" href="styles.css"/>
    <script src="https://cdn.jsdelivr.net/npm/chart.js@2.8.0/dist/Chart.min.js"></script>
</head>
<body>
    <h2 id="header-1">Average Annual Earth Temperature</h2> 
    <canvas id="chart" width="800" height="330"></canvas>

    <script>

        chartIt();
        async function chartIt() {
            const data = await getData();
            const ctx = document.getElementById('chart').getContext('2d');
            const myChart = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: data.xs,
                    datasets: [
                        {
                            label: 'Global Average Temp. in C° from 1880 through 2018',
                            data: data.ys,
                            fill: false,
                            backgroundColor: 'rgba(0, 165, 100, 0.2)',
                            borderColor: 'rgba(0, 165, 100, 1)',
                            borderWidth: 1
                        }
                    ]
                },
                options: {
                    scales: {
                        yAxes: [{
                            ticks: {
                                callback: function(value, index, values) {
                                    return value + '°';
                                }
                            }
                        }]
                    }
                }
            });
        }

        async function getData() {
            const xs = [];
            const ys = [];
            const response = await fetch('ZonAnn.Ts+dSST.csv');
            const data = await response.text();
            
            const table = data.split('\n').slice(1);
            table.forEach(elt => {
                const columns = elt.split(',');
                const year = columns[0];
                xs.push(year);
                const temp = columns[1];
                ys.push(parseFloat(temp) + 14);
                console.log(year, temp);
            }); 
            return { xs, ys };
        }
    </script>
</body>
</html>
