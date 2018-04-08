#### My issues
I have a web application and it went to LIVE mode. After this time, lot of issues happened because of user actions and server limitation. Although I fixed such issues, I want to monitor this server to detect similar ones and I have some ways to monitor server resources also. I researched at Internet and created my own monitor process. Now I will share with you how I manage my live server.

#### Solution
I separate my maintenance process into small ones. They are daily maintenance , weekly maintenance, monthly maintenance and bi-annual maintenance. Then I put monitor/maintenance tasks into appropriate categories based on how important of those tasks and how frequently those tasks should be made.

#### Daily maintenance
Here are some tasks I made daily. They take about 15 - 30 minutes to do

- Monitor Amazon SES (Simple Email Service): This is email service I used for my server. When used SES, I need to worry somethings.
  + SES limitation: SES allow us to send emails with limitations. For ex, it allows my server to send 750000 emails each 24 hour and 40 emails each second. Each day, I checked if limitation is reached or is nearly reached to request increase the limitation.

  + SES status: SES has some rules for sending email. They called "Bounce Rate" and "Complaint Rate". Bounce Rate is percentage of emails are sent by us to unreal email. Complaint Rate is percentage of emails are sent by us that marked as Spam. If one of two rules reaches a number, SES may suspend service. So I need to monitor them.

- Monitor server resources: This is important task I must made daily. I monitor network, CPU, Swap, Storage. If one criteria increases, I must find out why it happens. Most of reasons are new code have just deployed into server and create performance issue. BTW, I setup a notification: when each criteria is reach 70% of server limitation, email will be sent for us.

- Monitor scheduled processes: I want to know scheduled processes are running or not.

- Monitor slow requests: This is also a important task I must made daily. I used [NewRelic](http://newrelic.com/) (Paid Plan) to monitor server requests and detect slow ones

#### Weekly maintenance
Security is important thing we must care for a LIVE server. Weekly, I check security vulnerabilities of application with [acunetix](https://www.acunetix.com/) tool.

#### Monthly maintenance
- Check ROR updates/patches/vulnerabilities: Monthly, I checked if ROR release any version for updates/patches/vulnerabilities or not.

- Refresh server logs: In order to track user actions for better debugging, I keep all server logs (RoR, Sidekiq, Database, Cronjob, Nginx, Puma...). Monthly, I remove unused logs for saving cost

#### Bi-annual maintenance
- Change passwords/security tokens/ssh keys: To improve security of server, I change them bi-annually. Furthermore, I use [password generator](https://passwordsgenerator.net/) to generate strong passwords.

#### Others
Beside [NewRelic](http://newrelic.com/), I used other tools to monitor application

- Exception managements: I use [Rollbar](https://rollbar.com) to manage exceptions. This will also send email for me whenever exception raised.

- Uptime managements: To make sure server be always runninng, I used [Pingdom](https://www.pingdom.com/). This will send SMS for me whenever server is down.





