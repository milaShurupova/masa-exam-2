
# Masa exam: nodejs : Take 2

## Part I: Theory

*Please, do NOT use online resources as an assistance for this part of the exam*


## Section A: Please, select the correct answer or the most close to the correct one *=> 20 points:*

1. **What is Repro steps?** *=> 2 points*
 - [ ] Logical steps for planning the structure of DB repository
 - [+] Steps that are required in order to reproduce a bug in the system
 - [ ] Sequence of actions required to build a new nodejs server
 - [ ] A process of creation of a new Github repository

2. **What is the role of a helper in nodejs server architecture?** *=> 2 points*
 - [ ] Get the request from the router, treat the request parameters, prepare the response of the server to the consumer
 - [ ] Keep state of a specific logic portion of the system, provide processing of the data passed from the different controllers, parse the data and returned the processed response
 - [ ] To be the first element in the system that should service the consumer for his CRUD request to the server
 - [+] To provide services to the system that do not require a state but should be used across the whole system

3. **What is a foreign key?** *=> 2 points*
 - [ ] A key by which the JWT token is signed
 - [ ] A pair security key for a private key. This key is stored in online repository for asymmetric encryption processing
 - [+] A way to link between 2 tables in the relational DB
 - [ ] Such a term does not exist

4. **What is the code of a request redirection in HTTP protocol?** *=> 2 points*
 - [ ] 2xx
 - [+] 3xx
 - [ ] 4xx
 - [ ] 5xx

5. **What is a Jira ticket?** *=> 2 points*
 - [+] A definition of a task that should be done or details of a bug opened in the system
 - [ ] A ticket by which the symmtric authentication is being processed. This key is tored in the operating system
 - [ ] A portion of data used to authenticate against a nodejs server
 - [ ] A synonym for a JWT token

6. **Which npm package willl you use to get numerous help methods for arrays? (more than a single answer can be selected)** *=> 2 points*
 - [+] Underscore
 - [ ] Nodemon
 - [+] Lodash
 - [ ] All answers are correct

7. **What is needed for enhancement of a security level of a JWT token?** *=> 2 points*
 - [ ] Password
 - [ ] Encryption
 - [ ] A private key
 - [+] Secret

8. **What is dotenv?** *=> 2 points*
 - [ ] An environment of an operational system like release, production, etc.
 - [+] An npm package that provides the ability to set different system parameters for different system environments
 - [ ] A library of .Net nodejs environment
 - [ ] An npm package that is added by default while creating a new nodejs server

9. **How will you ensure a specific value in a DB column that is not the IDENTITY column will not be duplicate? (more than a single answer can be selected)** *=> 2 points*
 - [ ] Add another private key on this column
 - [+] Add a UNIQUE CONSTRAINT to this column
 - [+] Add a UNIQUE INDEX to this column
 - [ ] I will add a method on my server to ensure no duplicates are recorded in the DB for this column

10. **What is a log level** *=> 2 points*
 - [+] Log level is a level at which and up log messages will be recorded in any log target
 - [ ] It's a level of details to which a design of a task should be aligned
 - [ ] A level of DB logs storage to provide log shipping for a better system performance
 - [ ] There is no correct answer


## Section B: Please, explain the following terms the best way you can *=> 22 points*

(+) 11. **Constructor** *=> 4 points*

We use a function named constructor inside the class in order to create an instance of this class

(+) 12. **Connection string** *=> 4 points*

We use connection string in order to open connection to our database, it contains for example such info as server, database name, type of connection, driver

(+) 13. **IDENTITY & SEED** *=> 5 points*

In MS SQL we can use IDENTITY to make the column of the table to be the identifier for this table (in this case for each next line of data for this table, this column will be added +1 automatically)
SEEN is kinda entry (first) value for this IDENTITY column (from which table will start to count)

(+) 14. **Abstract class** *=> 5 points*

Abstract class is a class which doesnt have an instance; we use this class as a parent for other classes, who can have their instances.
In our code we create abstract class RouteConfig, and then based on him we create all other routes (SchoolRoutes, UserRoutes, AuthenticationRoutes) with super.

(+) 15. **Export keyword in nodejs code** *=> 4 points*

 We use export when we create different files and folders, and then we need to use functions, or classes, so on in another files of project.
 Using export, we can export the whole class to another module, or we can firstly create the instance and export the instance of the class itself, so from other module can`t be created another instance.
## Part II: Practice on paper *=> 45 points*

*No restrictions on online resources usage. You also may use your development machine for assistance in debugging if needed.*

(+) 16. **You received a bug stating the following: "Intermittemtly this method results in an exception in the command line reporting of a Null reference." You're required to find and fix the problem in this method:** *=> 10 points*

		public static addWeeks(date: Date, value: number): Date {
            date.setDate(date.getDate() + (value * 7));
            return date;
        }


public static addWeeks(date: Date, value: number): Date {

    let oldDate: Date = date;
    let oldDateMilSec: number = oldDate.valueOf();
    let valueMilSec: number = value * 7 * 24 * 60 * 60 * 1000;

    let newDate: Date = new Date(oldDateMilSec + valueMilSec);
    

    let expectedWeekDay: number = date.getDay();
    if (expectedWeekDay > 6) {
        expectedWeekDay = expectedWeekDay % 7;
    }

    if (expectedWeekDay < 0) {
        expectedWeekDay += 7;
    }


    const daysToAdd: number = newDate.getDay() >  expectedWeekDay ? -1 : 1;

    while (newDate.getDay() !== expectedWeekDay) {
        newDate.setDate(newDate.getDate() + daysToAdd);
    }

    return newDate;

}

(+) 17. **Having the following DB tables diagram:** *=> 10 points*

![](https://github.com/lentyaishe/masa-exam-2/blob/dev/resources/db-schema.jpg)

You need to write a query that returns for each user a full data. Consider that not every user in the system has an employee record. In such a case out put "Not an employee". Create By column should include the full name of the user that created the record of this an output user:

Full name	Created By	Position
John Doe	Mark Smith	Manager
Mary Smith	Klark Smith	Cashier
Patrice Raymond	Mark Smith	Not an eployee

SELECT (u1.first_name + ' ' + u1.last_name) AS 'Full name',
		(u2.first_name + ' ' + u2.last_name) AS 'Created By',
		CASE 
            WHEN e.position_id IS NULL THEN 'Not an employee'
            ELSE p.position_title 
        END AS 'Position'
FROM employee e
INNER JOIN [user] u1 ON u1.id = e.id
INNER JOIN [user] u2 ON u2.id = e.create_user_id
LEFT JOIN position p ON p.id = e.position_id

(+) 18. **Write a method in JS/TS that gets as an argument 2 dates and returns a boolean value of whether those dates share the same month**. *=> 5 points*

function compareMonth(date1: Date, date2: Date): boolean {

    let month1: number = date1.getMonth();
    let month2: number = date2.getMonth();
    let result: boolean;

    if (month1 === month2) {
        result = true;
    } else {
        result = false;
    }

    return result;

}

function compareMonthAndYear(date1: Date, date2: Date): boolean {

    let month1: number = date1.getMonth();
    let month2: number = date2.getMonth();
    let year1: number = date1.getFullYear();
    let year2: number = date2.getFullYear();
    let result: boolean;

    if (month1 === month2 && year1 === year2) {
        result = true;
    } else {
        result = false;
    }

    return result;

}

(+) 19. **Explain the following piece of code:** *=> 5 points* 

        public static dictionaryToArrayOfObjects<T>(input: _.Dictionary<T>): T[] {
            return _.map(Object.getOwnPropertyNames(input), (key: string) => input[key]);
        }

This is a generic method, which takes a dictionary as an input parameter, and returns an array of properties by these keys of this dictionary instead of dictionary (using underscore map method)

(+) 20. **Fix the following code and fill the required gaps in it by the coding standards. The purpose of this code is to verify the user is a member of a specific role and in case the user is, a true return value should be returned by the isUserPermitted() method, otherwise false. Treat the comments as actual code written that should not be changed.** *=> 15 points*

		interface user {
			id: number;
			firstName: string;
			lastName: string;
			roles: role;
		}
		
		interface dbUser {
			id: number;
			first_name: string;
			last_name: string;
			role_id: number;
			role_title
		}

		interface role {
			id: number;
			title: string;
		}

		enum Role {
			Administrator = 1,
			RegularUser = 2
		}
		
		public isUserPermitted(userId: number, roles: Role[]): Promise<bool> {
			return new Promise<bool>((resolve, reject) => {
				Promise.all([
					this.getUser(userId),
				])
				.then((user: dbUser) => {
					
					if (roles.indexOf(user.role_id) > -1) { 
						resolve(true)
					} else reject(false);
				}
					
				});
			});
		}
		
		private getUser(userId: number): Promise<user> {
			return new Promise<user>((resolve, reject) => {
				// Access to the DB that returns the user data by id as dbUser or null
			});
		}


## Part III: Practice on dev machine *=> 33 points*

*No restrictions on online resources usage*

 - Clone the repository: https://github.com/lentyaishe/masa-exam-2;
 - Switch to `broken-server` branch;
 - Create a new git repository under your personal Github called `masa-exam-2`;
 - Load the `main` branch of your new repository with the contents of the `broken-server` branch;
 - Each bug in the list should be solved in designated branch.

The following is the list of currently existing problems with the system. :

(+) 21. **The server solution cannot be compiled. Fix all compilation issues accordingly.** *=> 10 points*

(+) 22. **An attempt to start the server fails reporting "RangeError [ERR_SOCKET_BAD_PORT]: options.port should be >= 0 and < 65536. Received NaN".** *=> 8 points*

(+) 23. **A request to an endpoint GET <serverUrl:port>/user/-1 crashes the server with "ECONNRESET" error.** *=> 5 points* - Fixed so it returns 404 Not found if numeric parametr is less than 0

24. **Add the needed code in order to return a collection of teachers with the current `teacher` interface".** *=> 10 points*

# Good luck!!
