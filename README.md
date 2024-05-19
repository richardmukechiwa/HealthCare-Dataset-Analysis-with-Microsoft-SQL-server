# HealthCare-Database-Analysis-with-Microsoft-SQL-server
#### This project is about analysing a dataset about patients, medical conditions,  appointments, the  prescriptions and the respective doctors who attended to the appointments.
#### The analysis covers simple, intermediate and advanced SQL skills for example JOINS and  DATE FUNCTIONS .
#### The database was provided by Dr Eyo and this is the link to his X page where he talks about the free copy https://x.com/Eyowhite3/status/1726883774640562366

#### I am going to analyze the  10 questions below:

- Retrieve all appointments with their respective patient details (first name, last name).
- Find the total number of appointments made.
- List all doctors along with their specializations.
- Retrieve patients who are male and were born before 1980.
- Get the count of appointments for each doctor.
- Find doctors who prescribed medications and their respective medication count.
- List appointments scheduled for pediatricians.
- Identify patients who have appointments for child-related issues and the medications they are prescribed.
- Find doctors who prescribed medications that are expiring within the next 30 days.
- Retrieve patients who haven't had any appointments.

### The Project
#### Data Check and Manipulation
```
SELECT   *   FROM   dbo.Appointments;
```
```
SELECT   *   FROM   dbo.Doctors;
```
```
SELECT   *   FROM   dbo.Medications;
```
```
SELECT   *   FROM   dbo.Patients;
```

```
ALTER     TABLE      dbo.Medications
ALTER     COLUMN  PrescriptionDate  Date; --Changing datatype
```
```
ALTER     TABLE      dbo.Medications
ALTER     COLUMN  ExpiryDate  Date; -- Changing datatype
```
#### Analysis

##### Retrieve all appointments with their respective patient details (first name, last name).
```
SELECT        a.AppointmentID,
              a.AppointmentDateTime,
              a.PatientID,
              CONCAT(p.FirstName,' ',p.LastName) AS PatientName
FROM          dbo.Appointments AS a
LEFT JOIN     dbo.Patients     AS p
ON            a.PatientID = p.PatientID;
```
##### Outcome
![Hea 1](https://github.com/richardmukechiwa/HealthCare-Dataset-Analysis-with-Microsoft-SQL-server/assets/131812176/fa9cb545-2d41-49a9-982a-6e2c92cb8251)

###### Find the total number of appointments made.
```
SELECT        COUNT(DISTINCT AppointmentID) AS NumberofAppointments
FROM          dbo.Appointments;
 ```
##### Outcome
 ![Hea 2](https://github.com/richardmukechiwa/HealthCare-Dataset-Analysis-with-Microsoft-SQL-server/assets/131812176/8bcf89a7-3622-4eeb-aa9d-6cb67426b647) 
 
##### List all doctors along with their specializations.
```
SELECT        DoctorID,
              CONCAT(FirstName,' ',LastName) AS DocName,
              Specialisation
FROM          dbo.Doctors;
```
##### Outcome
![Hea 3](https://github.com/richardmukechiwa/HealthCare-Dataset-Analysis-with-Microsoft-SQL-server/assets/131812176/fa4e71a6-694f-4738-ba3e-03760b01661c)

##### Retrieve patients who are male and were born before 1980.
```
SELECT        CONCAT(FirstName,' ',LastName) AS PatientName,
              Gender,
              DateofBirth
FROM          dbo.Patients
WHERE         Gender = 'Male'
AND           DateofBirth < '1980'
ORDER  BY     DateofBirth ASC;
```
##### Outcome
![Hea 4](https://github.com/richardmukechiwa/HealthCare-Dataset-Analysis-with-Microsoft-SQL-server/assets/131812176/451e6a10-ceab-4ac6-b95f-2e1be9b875e7)

##### Get the count of appointments for each doctor.
```
SELECT        d.DoctorID,
              CONCAT(d.FirstName,' ',d.LastName) AS DocName,
n             COUNT(a.AppointmentID) AS AppointmentCount
FROM          dbo.Doctors AS d
LEFT  JOIN    dbo.Appointments AS a
ON            d.DoctorID = A.DoctorID
GROUP BY      d.DoctorID,
              FirstName,
              LastName;
```
##### Outcome
![Hea 5](https://github.com/richardmukechiwa/HealthCare-Dataset-Analysis-with-Microsoft-SQL-server/assets/131812176/bb6da9e6-494d-4610-9a6f-fcebe35d6276)
              
##### Find doctors who prescribed medications and their respective medication count.
```
SELECT        PrescribingDoctorID,
              CONCAT(FirstName,' ',LastName) AS DocName,
			  COUNT(DISTINCT MedicationID) AS MediCount
FROM          dbo.Doctors AS d
LEFT JOIN     dbo.Medications AS m
ON            d.DoctorID = m.PrescribingDoctorID
GROUP BY      PrescribingDoctorID,
              FirstName,
              LastName
ORDER BY      PrescribingDoctorID ASC;
```
##### Outcome
 ![Hea 6](https://github.com/richardmukechiwa/HealthCare-Dataset-Analysis-with-Microsoft-SQL-server/assets/131812176/03f03323-d38b-49d1-948c-f701dc0c32f8) 
 
##### List appointments scheduled for pediatricians.
```
SELECT        a.AppointmentID,
              a.AppointmentDateTime,
              a.DoctorID,
              CONCAT(d.FirstName,' ',d.LastName) AS DocName,
              Specialisation
FROM          dbo.Appointments AS a
LEFT   JOIN   dbo.Doctors AS d
ON            a.DoctorID = d.DoctorID
WHERE         Specialisation LIKE 'pediatric%';
```
##### Outcome
![Hea 7](https://github.com/richardmukechiwa/HealthCare-Dataset-Analysis-with-Microsoft-SQL-server/assets/131812176/4bff8ab9-c52a-4ff2-87f6-0efec38bd7e5)

##### Identify patients who have appointments for child-related issues and the medications they are prescribed.
```
SELECT        p.PatientID,
              CONCAT(p.FirstName,' ',p.LastName) AS PatientName,
              a.Purpose,
              m.MedicationName
FROM          dbo.Patients AS p
LEFT JOIN     dbo.Appointments  AS a
ON            p.PatientID = a.PatientID
LEFT JOIN     dbo.Medications AS m
ON            a.PatientID = m.PatientID
WHERE         Purpose LIKE '%child%';
```
##### Outcome
![Hea 8](https://github.com/richardmukechiwa/HealthCare-Dataset-Analysis-with-Microsoft-SQL-server/assets/131812176/e88c30a6-b924-4236-8705-d7f3e2c6382e)

##### Find doctors who prescribed medications that are expiring within the next 30 days.
```
SELECT         d.DoctorID,
               CONCAT(d.FirstName,' ',d.LastName) AS DocName,
               m.MedicationName,
               m.ExpiryDate
FROM           Doctors AS d
LEFT JOIN      Medications AS m
ON             d.DoctorID = m.PrescribingDoctorID
WHERE          ExpiryDate BETWEEN GETDATE() 
AND            DATEADD(Day, 30, GETDATE());
```
##### Outcome
![Hea 9](https://github.com/richardmukechiwa/HealthCare-Dataset-Analysis-with-Microsoft-SQL-server/assets/131812176/e355ee48-694a-4a6c-9ef5-8e627f075c49)

##### Retrieve patients who haven't had any appointments.
```
SELECT         p.PatientID,
               CONCAT(p.FirstName,' ',p.LastName) AS PatientName,
               a.AppointmentID
FROM           Patients AS p
LEFT  JOIN     Appointments AS a
ON             a.PatientID = p.PatientID
WHERE          AppointmentID IS NULL;
```
##### Outcome
![Hea 10](https://github.com/richardmukechiwa/HealthCare-Dataset-Analysis-with-Microsoft-SQL-server/assets/131812176/126dfb76-8d1c-4d94-ae3a-a8c8b9f65c66)







