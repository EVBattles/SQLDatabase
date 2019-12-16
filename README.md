# SQLDatabase
Database project for a hospital fundraiser foundation
CREATE SCHEMA donordata;

CREATE TABLE person
	(personID INT NOT NULL,
	personPrefix VARCHAR(3),
	personName VARCHAR(25) NOT NULL,
	personSuffex VARCHAR(3),
	personGender CHAR(1),
	personEmailAddress VARCHAR(50),
	personHomeAddress VARCHAR(50),
	personCity VARCHAR(12),
	personState CHAR(2),
	personHomePhone INT,
	personAlternatePhone INT,
	personBirthDate DATE,
	personStartDate DATE,
	groupsID INT,
CONSTRAINT personPK PRIMARY KEY (personID),
CONSTRAINT personFK FOREIGN KEY (groupsID) REFERENCES groups(groupsID));
INSERT INTO person VALUES
(001, 'Mr.', 'Santa Clause', 'PHD', 'M', 'sclause@hohoho.com', '1234 S. Reindeer Ln.', 'North Pole', 'AK', 1234567891, 1987654321, 19231225, 19551225, 002);
INSERT INTO person VALUES
(002, 'Mrs', 'Clara Clause', 'MD', 'F', 'cclause@hohohp.com', '1234 S. Reindeer Ln.', 'North Pole', 'AK', 1226667777, 1223334444, 19251225, 19541225, 002);
INSERT INTO person VALUES
(003, 'Mr.', 'Jesus Christ', 'PHD', 'M', 'jchrist@love.com', '777 Assention Ln', 'Heaven', 'UP', 1777777777, 1333333333, 00011225, 00341225, 003);
INSERT INTO person (personID, personName, personEmailAddress, groupsID) VALUES
(004, 'Jillian McDaniel', 'jillian.e.mcdaniel@biola.edu', 001);
INSERT INTO person (personID, personName, personEmailAddress, groupsID) VALUES
(005, 'Zachary Leonard', 'zachary.c.leonard@biola.edu', 001);
CREATE VIEW personInfoAll AS
SELECT personID, personPrefix, personName, personSuffex, personGender, personEmailAddress, personHomeAddress, personCity, personState, personHomePhone, personAlternatePhone, personBirthDate, personStartDate, groupsID
FROM person;
CREATE VIEW personInfoMinor AS
SELECT personID, personName
FROM person;
CREATE VIEW personContact AS
SELECT personID, personName, personEmailAddress, personHomeAddress, personCity, personState, personHomePhone, personAlternatePhone
FROM person;
CREATE VIEW personDemo AS
SELECT personID, personBirthDate, personGender, personCity, personState
FROM person;



CREATE TABLE donation
	(donationID INT NOT NULL,
	donationName VARCHAR(12) NOT NULL,
	moneyID INT,
	personID INT NOT NULL,
	donatedItemID INT,
CONSTRAINT donationPK PRIMARY KEY (donationID),
CONSTRAINT donationFK1 FOREIGN KEY (moneyID) REFERENCES money(moneyID),
CONSTRAINT donationFK2 FOREIGN KEY (personID) REFERENCES person(personID),
CONSTRAINT donationFK3 FOREIGN KEY (donatedItemID) REFERENCES donatedItem(donatedItemID));
INSERT INTO donation (donationID, donationName, personID, donatedItemID) VALUES
(001, 'toys', 001, 001);
INSERT INTO donation (donationID, donationName, personID, donatedItemID) VALUES
(002, 'cookies', 002, 002);
INSERT INTO donation (donationID, donationName, personID, donatedItemID) VALUES
(003, 'eternal life', 003, 003);
INSERT INTO donation (donationID, donationName, moneyID, personID) VALUES
(004, 'CASH DEPOSIT', 001, 002);
INSERT INTO donation (donationID, donationName, moneyID, personID) VALUES
(005, 'CASH DEPOSIT', 002, 001);
INSERT INTO donation (donationID, donationName, moneyID, personID) VALUES
(006, 'CASH DEPOSIT', 003, 001);
INSERT INTO donation (donationID, donationName, personID, donatedItemID) VALUES
(007, 'cake', 004, 004);
INSERT INTO donation (donationID, donationName, personID, donatedItemID) VALUES
(008, 'bread', 005, 005);
CREATE VIEW donateMoneyInfo AS
SELECT donation.personID, personName, donationID, donationName, donation.moneyID, moneyAmount
FROM person, donation, money
WHERE person.personID = donation.personID AND donation.moneyID = money.moneyID;
CREATE VIEW donateObjectInfo AS
SELECT donation.personID, personName, donationID, donationName, donatedItemCost
FROM person, donation, donatedItem
WHERE person.personID = donation.personID AND donation.donatedItemID = donatedItem.donatedItemID;



CREATE TABLE donatedItem
	(donatedItemID INT NOT NULL,
	donatedItemName VARCHAR(12) NOT NULL,
	donatedItemDescription VARCHAR(50),
	donatedItemCost INT,
	eventID INT,
CONSTRAINT donatedItemPK PRIMARY KEY (donatedItemID),
CONSTRAINT donatedItemFK1 FOREIGN KEY (eventID) REFERENCES event(eventID),
CONSTRAINT donatedItemFK2 FOREIGN KEY (donatedItemName) REFERENCES donation(donationName));
INSERT INTO donatedItem VALUES
(001, 'toys', 'toys for tots', 500, 001);
INSERT INTO donatedItem VALUES
(002, 'cookies', 'bake sale', 200, 002);
INSERT INTO donatedItem VALUES
(003, 'eternal life', 'resurrection', 999, 003);
INSERT INTO donatedItem VALUES
(004, 'cake', 'bake sale', 50, 002);
INSERT INTO donatedItem VALUES
(005, 'bread', 'bake sale', 30, 002);



CREATE TABLE money
	(moneyID INT NOT NULL,
	moneyAmount INT NOT NULL,
	moneyType VARCHAR(4) NOT NULL,
	eventID INT,
CONSTRAINT moneyPK PRIMARY KEY (moneyID),
CONSTRAINT moneyFK FOREIGN KEY (eventID) REFERENCES event(eventID));
INSERT INTO money VALUES
(001, 200, 'CASH', 001);
INSERT INTO money VALUES
(002, 100, 'CASH', 002);
INSERT INTO money VALUES
(003, 500, 'CASH', 002);
CREATE VIEW moneySum AS
SELECT SUM(moneyAmount)
FROM money;



CREATE TABLE event
	(eventID INT NOT NULL,
	eventName VARCHAR(20) NOT NULL,
	eventDate DATE,
CONSTRAINT eventPK PRIMARY KEY (eventID));
INSERT INTO event VALUES
(001, 'Toys for Tots', 20191225);
INSERT INTO event VALUES
(002, 'Winter Bake Sale', 20191225);
INSERT INTO event (eventID, eventName) VALUES
(003, 'Resurrection');
CREATE VIEW eventInfo AS
SELECT eventID, eventName, eventDate
FROM event;


CREATE TABLE groups
	(groupsID INT NOT NULL,
	groupsDescription VARCHAR(50),
	emailID INT NOT NULL,
	eventID INT,
CONSTRAINT groupsPK PRIMARY KEY (groupsID),
CONSTRAINT groupFK1 FOREIGN KEY (eventID) REFERENCES event(eventID),
CONSTRAINT groupFK2 FOREIGN KEY (emailID) REFERENCES email(emailID));
INSERT INTO groups VALUES
(001, 'Biola Grad', 001, 001);
INSERT INTO groups VALUES
(002, 'Christmas Giving', 002, 002);
INSERT INTO groups VALUES
(003, 'Resurrection', 003, 003);


CREATE TABLE email
	(emailID INT NOT NULL,
	emailName VARCHAR(20) NOT NULL,
	emailSaveDate DATE,
	emailText LONGTEXT NOT NULL,
	eventID INT,
CONSTRAINT emailPK PRIMARY KEY (emailID),
CONSTRAINT emailFK FOREIGN KEY (eventID) REFERENCES event(eventID));
INSERT INTO email VALUES
(001, 'Biola Giving', 20191209, 'Hello, Biola Grads. Here is an oportunity to give . . .', 001);
INSERT INTO email VALUES
(002, 'Christmas Giving', 20191209,'Hello, Christmas Givers. Here comes another Christmas . . .', 002);
INSERT INTO email VALUES
(003, 'Thank You', 20191209, 'Hello, Jesus. Thank you for the gift that you have given us. We remember you year round, but it is this season we tend to specifically focus on you . . .', 003);
CREATE VIEW emailInfo AS
SELECT emailID, emailName, emailSaveDate, emailText
FROM email;
