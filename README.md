# HealthCare-Database-Analysis-with-Microsoft-SQL-server
#### This project is about analysing a dataset about patients, medical conditions,  appointments, the  prescriptions and the respective doctors who attended to the appointments.
#### The analysis covers simple, intermediate and advanced SQL skills.
#### The database was provided by Dr Eyo and this is the link to his X page where he talks about the free copy https://x.com/Eyowhite3/status/1726883774640562366

#### I have come up with this questions for the analysis:

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

### The project

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

###### Find the total number of appointments made.
```
SELECT        COUNT(DISTINCT AppointmentID) AS NumberofAppointments
FROM          dbo.Appointments;
 ```
  
##### List all doctors along with their specializations.
```
SELECT        DoctorID,
              CONCAT(FirstName,' ',LastName) AS DocName,
			  Specialisation
FROM          dbo.Doctors;
```

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
##### Get the count of appointments for each doctor.
```
SELECT        d.DoctorID,
              CONCAT(d.FirstName,' ',d.LastName) AS DocName,
                            COUNT(a.AppointmentID) AS AppointmentCount
FROM          dbo.Doctors AS d
LEFT  JOIN    dbo.Appointments AS a
ON            d.DoctorID = A.DoctorID
GROUP BY      d.DoctorID,
              FirstName,
              LastName;
```
              
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

