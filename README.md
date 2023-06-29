# SerialMySQL

This project reads JSON-like data from a serial device (e.g., Calliope) and stores it in a MySQL database. It is designed for logging sensor readings or other structured device data.

## Features

* Connects to a serial port specified as a command-line argument.
* Reads JSON-like lines in the format `{id:<device>,<field>:<value>}`.
* Inserts parsed data into a MySQL database table.
* Handles serial port configuration (baud rate, timeouts, etc.).
* Continuously logs data until the program is terminated.

## Requirements

* C++ compiler (C++11 or later)
* MySQL server
* MySQL C++ Connector
* Linux environment with access to the serial port (`/dev/ttyACM*` or `/dev/ttyUSB*`)

## Database Setup

Create a database and table for storing the data:

```sql
CREATE DATABASE calliope;

USE calliope;

CREATE TABLE data (
    id INT AUTO_INCREMENT PRIMARY KEY,
    device_name VARCHAR(50),
    quantity VARCHAR(50),
    value VARCHAR(50),
    timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

## Usage

Compile the program:

```bash
g++ -o main main.cpp -lmysqlcppconn
```

Run the program with the serial port as an argument:

```bash
./main /dev/ttyACM0
```

### Input Format

The program expects lines like:

```
{id:device1,temper:31}
{id:device2,humidity:45}
```

* `id` — device identifier
* Other fields — values stored in the MySQL database (`quantity` and `value`)

### Output

* Data is inserted into the `data` table in MySQL.
* Errors are printed to the console.

## Serial Port Configuration

* Initial baud rate: 115200 (to wake up devices if needed)
* Operational baud rate: 9600
* Non-blocking read
* Read timeout: 1 second

## Notes

* Ensure the MySQL server is running and credentials are correct.
* The program runs continuously until manually stopped.

