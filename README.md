# Airline Booking & Operations Backend

A modular backend platform for managing airline booking, flight search, authentication, and operational notifications.

## �� Microservices

The system consists of 5 microservices:
- [Flights & Search Service](https://github.com/hussainaabid99/FlightsAndSearchService.git) (Current Repository) (Root) - Flight management, search, and airport operations
- [Auth Service](https://github.com/hussainaabid99/Auth_Service.git)- User authentication and authorization
- [Air Ticket Booking Service](https://github.com/hussainaabid99/AirTicketBookingService.git) - Ticket reservation and management
- [Reminder Service](https://github.com/hussainaabid99/ReminderService.git) - Automated notifications and reminders
- [API Gateway](https://github.com/hussainaabid99/API_Gateway.git) - Central routing and load balancing

## Flight & Search Service

## 🎯 Overview

The Flights & Search Service manages the complete flight catalog including cities, airports, airplanes, and flight schedules. It provides comprehensive search and filtering capabilities for flight discovery.

## ✨ Features

- **Flight Management**: CRUD operations for flight schedules
- **City & Airport Management**: Geographic location handling
- **Advanced Search**: Filter flights by price, airports, and dates
- **Data Validation**: Input validation and business logic
- **Flexible Filtering**: Query-based flight search

## 🛠️ Technology

- Node.js + Express.js
- Sequelize ORM
- MySQL database
- Input validation middleware
- Custom filtering utilities

## 🚀 Quick Start

```bash
npm install
# Create .env file with PORT
# Create src/config/config.json for database
npx sequelize db:create
npx sequelize db:migrate
npm start
```

## ⚙️ Configuration

### Environment Variables (.env)
```env
PORT=3000
SYNC_DB=true  # Optional: auto-sync database
```

### Database Configuration
Create `src/config/config.json`:
```json
{
  "development": {
    "username": "your_db_username",
    "password": "your_db_password",
    "database": "flights_service_dev",
    "host": "127.0.0.1",
    "dialect": "mysql"
  }
}
```

## �� API Endpoints

### Cities
- `POST /api/v1/city` - Create new city
  - Body: `{ "name": "New York" }`

- `GET /api/v1/city/:id` - Get city by ID
  - Response: City object with associated airports

- `GET /api/v1/city` - List all cities
  - Query: `?name=New` (optional name filter)

- `PATCH /api/v1/city/:id` - Update city
  - Body: `{ "name": "Updated City Name" }`

- `DELETE /api/v1/city/:id` - Delete city

### Airports
- `POST /api/v1/airports` - Create new airport
  - Body: `{ "name": "JFK", "address": "Queens, NY", "cityId": 1 }`

### Flights
- `POST /api/v1/flights` - Create new flight
  - Body: `{
    "flightNumber": "VS101",
    "airplaneId": 1,
    "departureAirportId": 1,
    "arrivalAirportId": 2,
    "departuretime": "2025-08-09T10:00:00Z",
    "arrivalTime": "2025-08-09T12:00:00Z",
    "price": 4500
  }`

- `GET /api/v1/flights` - Search flights
  - Query parameters:
    - `departureAirportId`: Departure airport ID
    - `arrivalAirportId`: Arrival airport ID
    - `minPrice`: Minimum ticket price
    - `maxPrice`: Maximum ticket price

- `GET /api/v1/flights/:id` - Get flight details
- `PATCH /api/v1/flights/:id` - Update flight information

## ��️ Database Models

### City Model
```javascript
{
  id: INTEGER (Primary Key),
  name: STRING (Unique),
  createdAt: DATE,
  updatedAt: DATE
}
```

### Airport Model
```javascript
{
  id: INTEGER (Primary Key),
  name: STRING,
  address: STRING,
  cityId: INTEGER (Foreign Key to City),
  createdAt: DATE,
  updatedAt: DATE
}
```

### Flights Model
```javascript
{
  id: INTEGER (Primary Key),
  flightNumber: STRING (Unique),
  airplaneId: INTEGER,
  departureAirportId: INTEGER,
  arrivalAirportId: INTEGER,
  departuretime: DATE,
  arrivalTime: DATE,
  price: INTEGER,
  boardingGate: STRING,
  totalSeats: INTEGER,
  createdAt: DATE,
  updatedAt: DATE
}
```

## �� Search & Filtering

### Flight Search Filters
- **Airport-based**: Filter by departure/arrival airports
- **Price Range**: Filter by minimum and maximum price
- **Combined Filters**: Multiple filter criteria support

### Example Search Queries
```bash
# Flights from airport 1 to airport 2
GET /api/v1/flights?departureAirportId=1&arrivalAirportId=2

# Flights within price range
GET /api/v1/flights?minPrice=1000&maxPrice=5000

# Combined filters
GET /api/v1/flights?departureAirportId=1&minPrice=2000&maxPrice=8000
```

## ✅ Validation

### Flight Creation Validation
- All required fields must be present
- Arrival time must be after departure time
- Flight number must be unique
- Price must be positive integer

### Business Logic
- Time validation ensures logical flight schedules
- Airplane capacity determines total seats
- Airport relationships maintain data integrity

## 🚀 Future Improvements

- Flight availability checking
- Seat inventory management
- Real-time pricing updates
- Flight status tracking
- Route optimization
- Performance analytics

## 📝 Development Notes

- Cities have unique names
- Airports belong to cities (one-to-many)
- Flights connect airports (many-to-many through flights)
- Price filtering supports both min and max values
- Search is case-insensitive for city names
