---
title: '"Parking Lot low Level Design"'
draft: 
tags:
  - LLD
  - ParkingLot
  - OOPS
  - DesignPrinciples
  - component
  - collegestudents
---
**Parking Lot Design: Navigating the Tech Interview Maze**

  

I've been diving into the world of tech interviews lately, and I've got to admit, it's been a bit disappointing. It seems like companies are all following the same interview trends, and "content creators" are cashing in on this pattern. I decided to check out Agoda's interview experiences on LeetCode, and they stood out with their well-structured approach.

  

What really caught my attention were the Low-Level Designs (LLDs). I've never really delved into them from an interview perspective before. While exploring, I stumbled upon a bunch of videos on how to tackle seemingly simple problems like designing a parking lot. Some folks nailed it with a smart and sensible approach, while others seemed to throw every coding pattern they knew at the problem.

  

The issue here is that newcomers often end up memorizing solutions to specific problems without really understanding the core concepts. Striking a balance between problem-solving finesse and grasping the basics is key.

  

In interviews, companies may require diagrams or code. We'll explore creating both without overusing design patterns and emphasize crucial interview points.


# **Requirement Gathering.** 

  
One of the major points in both LLD and HLD phases. If you can master this art, you can navigate a lot of projects in your company as well. 

  

“Imagine you're tasked with designing a parking lot management system. How would you gather requirements for this project?”

  

I'll avoid sticking to the typical interview pattern and help us grasp the essence of it. This phase is crucial from a developer's viewpoint as it demonstrates the ability to think beyond coding—a key quality sought after by top companies in their developers.

  

- Functional Requirements: Define how vehicles enter, exit, reserve spots, pay fees, and track occupancy.
- Input and Output: Capture vehicle information upon entry, issue parking tickets, accept payments, and provide receipts upon exit.
- Edge Cases and Error Handling: Handle scenarios like a full lot, lost tickets, and system failures gracefully.
- Performance Considerations: Optimize for high-volume usage during peak hours, ensuring efficient entry and exit processes.
- Security: Implement user authentication, data protection, and surveillance to safeguard the parking lot.
- Data Management: Store and manage vehicle data, reservations, and payments securely with data validation and backups.
- Usability: Design user-friendly interfaces, clear signage, and conduct usability testing for a seamless experience.
- Testing: Develop and execute tests, including unit tests and load testing, to ensure reliability and performance.
- Maintenance: Plan for ongoing maintenance, bug fixes, security updates, and potential future enhancements.

  

  

These points represent the foundation of software development. Interviews may emphasize specific aspects, but understanding the entire development pipeline is crucial. While there are many books on requirement gathering, let's focus on these essentials for now.

  

# **Functional Requirements**

  

In an interview or when engaging with stakeholders, asking and clarifying functional requirements is crucial to understand the project's scope. Using the example of a parking lot system:

- **Ask Open-Ended Questions**: Begin by asking open-ended questions to gather initial insights. For instance, you can ask, "Could you describe how vehicles should interact with the parking lot system?"
- **Identify Core Functionalities**: Dive deeper to identify the core functionalities. Ask questions like, "Should the system allow vehicles to reserve parking spaces in advance?" or "What are the key actions users should perform, such as entry, exit, or payment?"
- **Discuss User Roles**: Clarify who the primary users are and their roles. Ask, "Are there different types of users, like customers, parking attendants, or administrators? What tasks do they need to perform?"
- **Capture Specifics**: Request specifics about each functionality. For example, "When a vehicle enters, should it provide its license plate number, and should the system issue a digital or printed parking ticket?"
- **Handle Exceptional Cases**: Inquire about exceptional scenarios. Ask, "What happens if the parking lot is full, or if a user loses their parking ticket? How should these situations be handled?"
- **Payment and Fee Calculation**: If payment is involved, clarify how it should work. Ask, "What payment methods should be supported, and how should parking fees be calculated – hourly, daily, or otherwise?"
- **Real-Time Monitoring**: If relevant, discuss real-time monitoring requirements. Ask, "Should the system provide real-time information on available parking spaces?"

  

By asking these questions and seeking clarification, you demonstrate your ability to understand and document functional requirements effectively. This approach ensures that you and the stakeholders are on the same page and helps prevent misunderstandings. 

  

**Determining the domain models**

  

Certainly! Let's walk through the process of identifying domain models, determining classes, and defining their methods for a parking lot system:

  

**1. Understand the Problem Domain:**

   - We want to create a system to manage a parking lot. This includes vehicles entering and exiting, reserving spots, issuing tickets, processing payments, and monitoring occupancy.

  

**2. Identify Nouns and Verbs:**

   - Nouns: Vehicle, ParkingSpot, Ticket, Payment, User.

   - Verbs: Enter, Exit, Reserve, CalculateFee, Validate.

  

**3. Create an Initial List of Classes:**

   - Vehicle

   - ParkingSpot

   - Ticket

   - Payment

   - User

  

**4. Define Class Attributes:**

  

   - Vehicle:

     - Attributes: licensePlate, entryTime, exitTime

  

   - ParkingSpot:

     - Attributes: spotNumber, isOccupied, vehicle (to store the parked vehicle)

  

   - Ticket:

     - Attributes: ticketNumber, entryTime

  

   - Payment:

     - Attributes: paymentMethod, amount

  

   - User:

     - Attributes: username, password, role

  

**5. Determine Class Methods:**

  

   - Vehicle:

     - Methods: enterParkingLot, exitParkingLot

  

   - ParkingSpot:

     - Methods: occupy, vacate

  

   - Ticket:

     - Methods: generateTicket

  

   - Payment:

     - Methods: processPayment

  

   - User:

     - Methods: login, logout

  

**6. Establish Relationships:**

  

   - Vehicle has a relationship with Ticket (one-to-one), as each vehicle corresponds to a ticket.

   - ParkingSpot has a relationship with Vehicle (one-to-one or one-to-many), indicating which vehicle is parked there.

   - User may have a relationship with Ticket (one-to-many), as users may have multiple parking tickets.

  

**7. Refine and Review:**

   - Share your class definitions and relationships with stakeholders or peers to ensure they align with the parking lot system's requirements.

  

**8. Consider Inheritance and Interfaces:**

   - You might consider creating a common interface or base class for objects that require payment, like Ticket and Payment, to share payment-related methods.

  

**9. Document Your Domain Model:**

   - Create a visual representation (e.g., a UML diagram) to document your domain model, including classes, attributes, methods, and relationships.

  

**10. Iterate and Test:**

   - As you work on the project, be open to making adjustments to your domain model based on evolving requirements or feedback from testing.

  

This step-by-step process helps you systematically define the domain models, classes, attributes, and methods for your parking lot system, ensuring clarity and alignment with the project's goals.

# Java Code 
