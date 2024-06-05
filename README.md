# DATABASE DESIGN for BUS ROUTE SCHEDULE SYSTEM

### Overview
- This project is a great experience for me to have a general look from top to down at the database design process.
- During the implementation, I have figured out another approach of the datase design process for this case.
- The new approach shows a shift from production-oriented mindset to market-oriented mindset that I will explain in more details below. 

### Problem Statement:
The task is to design and implement a Bus route schedule system. It needs to keep record of the following:
- **Drivers**: You should be able to keep record about the bus drivers and their information including their driver's license and its expiry date.
- **Busses**: You should keep record of the busses in the fleet and some important information as number of passengers and license plate expiry date.
- **Customers**: You need to keep important customer info as they will purchase subscriptions. The info to be stored is up to your assumption on what's necessary.
- **Subscription**: You should keep track of the subscription to the bus system such as monthly or some balance for rides. You will also need to keep track of the rides on each subscription. This means when a customer uses a bus under a certain scubscription, the customer ID, subscription ID and the bus ID need to be stored.
- **Routes**: You should keep track of the routes that the bus go through the city including the stops in the route and which busses go on that route.
- **Stations**: These are the bus stations which can be a bus stop too but it has the offices to buy bus tickets or subscriptions.
  
### Task:
- Design an ERD for the above problem
- Convert ERD to Relational Model
- Implement the database and populate the tables with data
- Write queries to accomplish the some task:
1. View all routes in the city and what busses go on these routes.
2. Search by bus stop to see which bus goes to that stop and display the bus number and the full route.
3. Display driver's information and the assigned bus.
4. Search for expired license plates for the busses and/or expired driver's license and display the bus information and the driver's information.
5. Search by station and see all busses stopping at that station along with the routes they will go on.
6. View all busses and all routes for the busses.
7. Search by customers and show their subscription information.

----------------------------------------------------------------------------------------------------------------------------------------------------
## SOLUTION

### New Problem Approach:
- During the process of transfering Business Rules into ERD Diagram, I realized that I was doing the process without knowing what I was doing for. Then, I changed the process of database design, so that everything could become more smoothly for my thought in this case. Please view the visualizations here for better clarification.
<div align="center">
<img width="600" alt="Database Design Process" src="https://github.com/keasark/bus-system-database-design/assets/76891395/56ae2699-a765-4ac1-b165-0592c220bf90">
</div>
<br>
- I think those 2 mindsets (mentioned in the above images) applied for the case of Software Development Life Cycle (SDLC) and Rapid Application Development (RAD) (particularly, Prototyping) processeses too. 

### Logic of the New Approach:
- Applying the Market-oriented mindset, I started first with the assumption what the bus system owner really needs. There are 2 main characters: drivers and passengers. Drivers may need bus tracking interface. Passengers may need bus tracking and purchase confirmation interfaces. From there, the bus owner can point out a GUI design he wants.
- Each interface like Associative Entities often includes various attributes coming from different entities. That’s why I could do Normalization process to break those Associate Entities into normalized entities. Then, I continued ERD model with relationships.
<div align="center">
<img width="500" alt="Capture" src="https://github.com/keasark/bus-system-database-design/assets/76891395/dac1fa82-37fe-4882-b7d2-f760fea2f2cc">
</div>
<br>

### Business Demand:
- Design and implement a Bus route schedule system in order to program interactive applications for 2 main users: drivers and customers/passengers.
<div align="center">
<img src="https://github.com/keasark/bus-system-database-design/assets/76891395/64b23d1a-495b-45ac-a760-14a74df31030"/>
</div>

### User Views:
The context only focuses on 2 main users: drivers and passengers. For different views of users, we will have different GUI design. 
- Driver’s view: the drivers need to know which route, bus, stops about a given route they get assigned in a given date and time. Also, the drivers need to double check customers about their personal info, route and payment confirmation before allowing them to get in the bus.
- Customer’s view: after buying a bus trip, a customer should receive a confirmed receipt about their upcoming bus trip, such as route info, selling location and their approved payment. So, they can track their trip and show the receipt to a driver for proving their authorization. 

### GUI Design:
1 application includes different interfaces based on various user’s views.
- Login interface: can be used for both drivers and customers but linked to different pages later related to their login authorization.
<br>
<div align="center">
<img src="https://github.com/keasark/bus-system-database-design/assets/76891395/af651b9f-312d-46fb-9f7e-feb687414f8f" width="200" height="400" />
</div>
<br>
- Route interface: drivers can use Route page to track their current trip about the destination, bus, stops and upcoming passengers. Customers / Passengers can also use the Route page for tracking their trip but they are not allowed to see the bus or driver’s detailed info. For this project, we only design Route page for drivers.
<br>
<div align="center">
<img src="https://github.com/keasark/bus-system-database-design/assets/76891395/f9d78182-f1a5-4e46-87ca-2110fb2ca81e" width="200" height="400" />
</div>
<br>
- Purchase interface: customers can access a confirmed receipt from here where they can show it to a driver for their authorization to get in a bus. They can see info about a bus route they purchased, from which station - ticket box, and by their approved payment. Drivers are not allowed to see customers’ detailed payment info, so actually they don’t need this Purchase interface.
<br>
<div align="center">
<img src="https://github.com/keasark/bus-system-database-design/assets/76891395/2b9249a3-243f-4c77-9ae5-2d2356f9d959" width="200" height="400" />
</div>
<br>

### Assumptions:
- Ticket box in station has selling functionality for paper/electronic tickets, subscription, even deposit bus account balance.
- Ticket box is a self-service machine which needs no staff to operate.
- There might have multiple ticket boxes placed in a station. The location of ticket box is decided by where it’s placed, e.g. food center, front door, etc.
- Tickets sold for customers under paper/electronic version are One-time purchase. Similarly, Amazon products can be bought by one-time payment or subscriptions.
- Bus system sells their bus route under 3 payment methods which are tickets, balance and monthly subscription. Therefore, customers’ payment is classified into those 3 categories.
- Route is a single bus trip. Route = Trip = Ride.

### ERD Logic:
- Two main associative entities, which are Route entity and Purchase entity, represent for Route interface and Purchase interface explained in GUI design part.
- Route entity connecting bus, driver, stop, customer shows the necessary access authorization provided for drivers and customers in their Route interface. Correspondingly, Purchase entity connecting customer, payment, station, ticket-box shows that the receipt customers access in Purchase interface is able to show that info.
- For the relationship between Ticket Box and Payment, ticket box sells the route for customers and get payment from them. A Ticket Box can issue 0 to many payments and payment can pay 0 to multiple times (ticket, deposit balance, subscription) at the same ticket box. The result of their relationship, shown by Purchase associative entity – Receipt issued for customers, represents for the Ticket Box’s selling functionality. Also, Purchase / Receipt needs to confirm the upcoming route for customers, that’s why Purchase entity connects with Route entity.

