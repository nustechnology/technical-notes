#### My issues

I have a project, it's written by Ruby on Rails. I used MongoDB for DBMS and AWS as hosting service. This is tracking application has two main purposes: The first one is receive tracking data from client and store them. The second one is present data to user. A tracking data module is heavy processing module (with more than 1000 request each month). I realize some issues with tracking data module

+ Performance: As default, Puma server only opens 32 connections as maximum. This is not suitable to such amount of request come to tracking data module.
+ Maintenance: I cannot restart server (for update new code for instance). Some tracking request cannot be processed at start-up time.

Finally, I decided that above issues are structured-level issues of my application structure: Monolithic. Let's discuss more detail about it

#### Monolithic hell

Monolithic is traditional structure. With this structure, we will write all of your code into a single project. What is the benefit of it? Firstly, it's simple and traditional. They are simple to develop and IDE supported. Furthermore, it's easy for deploying and integrating.

So, what is the weakness of Monolithic structure? All components are interconnected and interdependent. It's tightly-coupled architecture, each component and its associated components must be present in order for code to be executed or compiled. If one component has a error, whole application is crashed. Sometime, my application cannot be started because developer misses some files when pushing code.
In theory, my tracking application has two independent modules. But they're not independent as code base structure. It's caused by Monolithic structure
Another weakness of Monolithic structure that we put all of the code into single project and it grows up day to day. And some years later, we will realize that project is too big and complex. It's painful to debug or maintenance. It's nightmare for developer working on this project. And with a lot of code, it's also take more time for compiling and starting. It's actually the big trouble. What did I do with to solve my issues? Break my application out of some peaces with Micro-service structure concept!!!

#### Micro-services, my solution

Micro-services is new structure that we will separate our code into some independent services. Each micro-service is a mini‑application.

In my case, I separated tracking server out of current project and deploy it into a Rack application server, it has 1024 connections as default and it solves my first issue immediately!!! Now, two modules of my application are really independent. When updating code for Rails server, I restart Puma server, and when updating code for tracking server, I restart Rack server (rarely).

One of benefit of Micro-services, I can totally use another framework like PHP or NodeJS to implement tracking server. This is the action that Monolithic cannot made (never). Furthermore, it's easy to optimize environments (RAM, CPU, server...)

However, Micro-services is not Monolithic replacement. It also contains lot of issues:

- It's micro-services. So, it means we must separates code into some small parts. So it's difficult to manage than Monolithic. One more thing, if we have some global change, we must apply as many places. For example, sometime, I (and my team) forgot to update Model code of tracking server although I (and they) updated Model code of Rails server.

- In Micro-services is independent services, so if they want to contact each others, we must implement a API. It takes effort than Monolithic: all services can see each others easily.

- It's difficult of testing also.

- It's kind of distributed system so it contains distributed system issues: consistency, availablity, partition tolerance.

- It's also double my deployment effort.

So, please consider Micro-services as another structure for your application. If your project is simple, just use Monolithic structure. But think carefully about project development in the future or you will deep into the "Monolithic Hell". The Micro-services architecture pattern is the better choice for complex, evolving applications despite the drawbacks and implementation challenges.
