# Part I - Models
## models / flights.js
```javascript
let flights = [];

function getAllFlights() {
    return flights;
}

function addFlight(flightData) {
    flights.push(flightData);
}

module.exports = { getAllFlights, addFlight };
```

# Part II - Views
## addFlight.ejs
```ejs
<!DOCTYPE html>
<html>
<head>
    <title>Add Flight</title>
</head>
<body>
    <h1>Add New Flight</h1>
    <form action="/flights/add" method="post">
        <div>
            <label for="flightNumber">Flight Number:</label>
            <input type="text" id="flightNumber" name="flightNumber" required>
        </div>
        <div>
            <label for="source">Source:</label>
            <input type="text" id="source" name="source" required>
        </div>
        <div>
            <label for="destination">Destination:</label>
            <input type="text" id="destination" name="destination" required>
        </div>
        <button type="submit">Add Flight</button>
    </form>
    <a href="/flights">Back to Flights List</a>
</body>
</html>
```


## index.ejs
```ejs
<!DOCTYPE html>
<html>
<head>
    <title>Flight List</title>
</head>
<body>
    <h1>Flight List</h1>
    <% if(flights.length > 0){ %>
        <ul>
            <% flights.forEach(function(flight) { %>
                <li><%= flight.flightNumber %> - <%= flight.source %> to <%= flight.destination %></li>
            <% }); %>
        </ul>
    <% } else { %>
        <p>No flights added yet.</p>
    <% } %>
    <a href="/flights/add">Add New Flight</a>
</body>
</html>
```

# Part III - Controller
## controllers/flightController.js
```javascript
// -----------------------------------------------
// controllers/flightController.js
// -----------------------------------------------

const FlightModel = require('../models/flight');

exports.addFlightForm = (req, res) => {
    res.render('addFlight');
};

exports.addFlight = (req, res) => {
    FlightModel.addFlight(req.body);
    res.redirect('/');
};

exports.listFlights = (req, res) => {
    let flights = FlightModel.getAllFlights();
    res.render('index', { flights });
};
```

# Route
## routes/flight.js
```javascript
// --
// routes/flight.js
// --

var express = require('express');
var router = express.Router();
var flightController = require('../controllers/flightController');

router.get('/add', flightController.addFlightForm);
router.post('/add', flightController.addFlight);
router.get('/', flightController.listFlights);

module.exports = router;
```

# Part IV - Adopt your new Component
## app.js
```javascript
// ---
// app.js
// ---
var flightRouter = require('./routes/flight');
...
app.use(express.urlencoded({ extended: false }));
...
app.use('/flights', flightRouter);
```
