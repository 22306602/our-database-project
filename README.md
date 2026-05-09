CREATE TABLE plane_model (
    model_id INT AUTO_INCREMENT PRIMARY KEY,
    model_no VARCHAR(30) UNIQUE NOT NULL,
    manufacturer VARCHAR(50) NOT NULL,
    max_range INT,
    fuel_capacity INT
);

INSERT INTO plane_model(model_no, manufacturer, max_range, fuel_capacity)
VALUES
('A320', 'Airbus', 6100, 27000),
('B737', 'Boeing', 5600, 26000);

 select * from plane_model;

CREATE TABLE airplane (
    plane_id INT AUTO_INCREMENT PRIMARY KEY,
    plane_no VARCHAR(20) UNIQUE NOT NULL,
    model_id INT NOT NULL,
    capacity INT NOT NULL,
    manufacture_year YEAR,
    status VARCHAR(20),

    FOREIGN KEY (model_id)
    REFERENCES plane_model(model_id)
);

INSERT INTO airplane(plane_no, model_id, capacity, manufacture_year, status)
VALUES
('TC-AAA', 1, 180, 2018, 'Active'),
('TC-BBB', 2, 220, 2020, 'Maintenance');

 select * from airplane;

CREATE TABLE hangar (
    hangar_id INT AUTO_INCREMENT PRIMARY KEY,
    hangar_no VARCHAR(10) UNIQUE NOT NULL,
    location VARCHAR(100),
    capacity INT
);

INSERT INTO hangar(hangar_no, location, capacity)
VALUES
('H1', 'North Zone', 5),
('H2', 'South Zone', 7);

 select * from hangar;

CREATE TABLE hangar_history (
    history_id INT AUTO_INCREMENT PRIMARY KEY,
    plane_id INT NOT NULL,
    hangar_id INT NOT NULL,
    in_datetime DATETIME NOT NULL,
    out_datetime DATETIME,

    FOREIGN KEY (plane_id)
    REFERENCES airplane(plane_id),

    FOREIGN KEY (hangar_id)
    REFERENCES hangar(hangar_id)
);

INSERT INTO hangar_history
(plane_id, hangar_id, in_datetime, out_datetime)
VALUES
(1, 1, '2026-05-01 08:00:00', '2026-05-03 14:30:00'),
(2, 2, '2026-05-02 10:15:00', NULL),
(1, 2, '2026-04-20 09:00:00', '2026-04-22 16:45:00');

 select * from hangar_history;

CREATE TABLE test (
    test_id INT AUTO_INCREMENT PRIMARY KEY,
    test_name VARCHAR(50) NOT NULL,
    description TEXT,
    max_score INT
);

INSERT INTO test(test_name, description, max_score)
VALUES
('Engine Test', 'Checks aircraft engine condition', 100),
('Hydraulic Test', 'Checks hydraulic systems', 100);

 select * from test;

CREATE TABLE employee (
    ssn VARCHAR(20) PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    phone VARCHAR(20),
    hire_date DATE,
    salary DECIMAL(10,2),
    union_membership_no VARCHAR(30) UNIQUE
);

INSERT INTO employee(ssn, first_name, last_name, phone, hire_date, salary, union_membership_no)
VALUES
('1001', 'Ali', 'Hasan', '05338888888', '2020-03-01', 5000, 'U100'),
('1002', 'Mehmet', 'Kaya', '05339999999', '2019-06-15', 6500, 'U101');

 select * from employee;

CREATE TABLE technician (
    technician_id INT AUTO_INCREMENT PRIMARY KEY,
    ssn VARCHAR(20) NOT NULL,
    specialization_level VARCHAR(30),

    FOREIGN KEY (ssn)
    REFERENCES employee(ssn)
);

INSERT INTO technician(ssn, specialization_level)
VALUES
('1001', 'Senior');

 select * from technician;

CREATE TABLE technician_expertise (
    technician_id INT NOT NULL,
    model_id INT NOT NULL,

    PRIMARY KEY (technician_id, model_id),

    FOREIGN KEY (technician_id)
    REFERENCES technician(technician_id),

    FOREIGN KEY (model_id)
    REFERENCES plane_model(model_id)
);

 select * from technician_expertise;

CREATE TABLE test_event (
    event_id INT AUTO_INCREMENT PRIMARY KEY,
    plane_id INT NOT NULL,
    technician_id INT NOT NULL,
    test_id INT NOT NULL,
    test_date DATE,
    hours_spent DECIMAL(4,2),
    score INT,

    FOREIGN KEY (plane_id)
    REFERENCES airplane(plane_id),

    FOREIGN KEY (technician_id)
    REFERENCES technician(technician_id),

    FOREIGN KEY (test_id)
    REFERENCES test(test_id)
);

INSERT INTO test_event(plane_id, technician_id, test_id, test_date, hours_spent, score)
VALUES
(1, 1, 1, '2026-05-01', 3.5, 95);

 select * from test_event;

CREATE TABLE traffic_controller (
    controller_id INT AUTO_INCREMENT PRIMARY KEY,
    ssn VARCHAR(20) NOT NULL,
    last_medical_exam DATE,

    FOREIGN KEY (ssn)
    REFERENCES employee(ssn)
);

INSERT INTO traffic_controller(ssn, last_medical_exam)
VALUES
('1002', '2025-01-10');

 select * from traffic_controller;

CREATE TABLE flight (
    flight_id INT AUTO_INCREMENT PRIMARY KEY,
    flight_no VARCHAR(20) UNIQUE NOT NULL,
    plane_id INT NOT NULL,
    departure_city VARCHAR(50),
    arrival_city VARCHAR(50),
    departure_time DATETIME,
    arrival_time DATETIME,

    FOREIGN KEY (plane_id)
    REFERENCES airplane(plane_id)
);

INSERT INTO flight
(flight_no, plane_id, departure_city, arrival_city, departure_time, arrival_time)
VALUES
('CY101', 1, 'Nicosia', 'Istanbul',
 '2026-05-10 08:00:00',
 '2026-05-10 09:30:00'),

('CY202', 2, 'Nicosia', 'Ankara',
 '2026-05-11 13:15:00',
 '2026-05-11 14:45:00'),

('CY303', 1, 'Nicosia', 'London',
 '2026-05-12 06:00:00',
 '2026-05-12 11:30:00');
 
 select * from flight;
