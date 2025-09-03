<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8" />
  <link href="https://cdn.jsdelivr.net/npm/fullcalendar@6.1.10/index.global.min.css" rel="stylesheet">
  <script src="https://cdn.jsdelivr.net/npm/fullcalendar@6.1.10/index.global.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.4.1/papaparse.min.js"></script>
  <style>
    body { font-family: Arial, sans-serif; margin: 20px; }
    .fc-booked { background-color: #ff9999 !important; }       /* red for booked */
    .fc-available { background-color: #b3ffb3 !important; }   /* green for available */
    .fc-changeover {
      background: linear-gradient(to bottom, #ff9999 50%, #b3ffb3 50%) !important;
    }
  </style>
</head>
<body>
  <div id="calendar"></div>

  <script>
    document.addEventListener('DOMContentLoaded', function() {
      var calendarEl = document.getElementById('calendar');
      var calendar = new FullCalendar.Calendar(calendarEl, {
        initialView: 'dayGridMonth',
        events: function(fetchInfo, successCallback, failureCallback) {
          Papa.parse("https://docs.google.com/spreadsheets/d/e/2PACX-1vRPwBmfzZ0LW5R2l9w5WUWrhIycY1i_cutGW01j57ecPSbvTr6vmBWB_Wdxdocv3T1sNnmCLi6HULjI/pub?gid=0&single=true&output=csv", {
            download: true,
            header: true,
            complete: function(results) {
              let events = results.data.map(row => {
                let cssClass = '';
                if (row.Status === 'Booked') cssClass = 'fc-booked';
                else if (row.Status === 'Available') cssClass = 'fc-available';
                else if (row.Status === 'Changeover') cssClass = 'fc-changeover';

                return {
                  title: row.Status + (row.Price ? " – " + row.Price : ""),
                  start: row.Date,
                  className: cssClass
                };
              });
              successCallback(events);
            }
          });
        }
      });
      calendar.render();
    });
  </script>
</body>
</html>
