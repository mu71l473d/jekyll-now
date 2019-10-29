---
layout: post
title: Kiwa Workshop
---

It is now time to start. But first of all:

#### [Rules Kiwa Workshop] (# rules)
- You have until 14:45 to solve the challenges. Then I will take over and do some closing words
- You are not allowed to do DoS / DDoS the Kiwa Workshop
- You are not allowed to view, edit or attack instances of other teams. Other ways of harassing other teams are also not permitted.
- The source code of bkimminich and mu71l473d are off limits during the challenge, and the search for answers is also not allowed.
- The use of a vulnerability scanner is not intended use. You are supposed to search for vulnerabilities yourself.
- You may not modify the database so that you have solved all challenges.
- Feel free to ask questions! The game leader (Rene Bisperink) does try to not give away too much.
- It is also allowed to ask your team member or the game leader for hints or help.
- You can search for general information about security tests and hacking methods, just searching for specific answers for the challenges or browsing the source code is off-limits.
- Last but not least: Have fun!

#### [Other notes] (#notes)
- the Kiwa workshop is supposed to be the opposite of a best practice or template application for web developers: It is an awareness, traning, demonstration and exercise tool for security risks in modern web apps.
- Challenges up to three stars are all tested and confirmed working. However, if a challenge doesn't register correctly, it can be useful to go back to the main menu and back to the home page and then back to the exploited page for it to register correctly.
- Even if the solutions for the workshop are found, Rene modified the challenges to not all be the answers as documented.
- If teams get to the four stars challenges in 2.5 hours we will start to question the team composition or the finding of answers of the solution(s) ;) It is also possible that the team is just very good.
- Rene's main expertise is hacking and penetration testing, not specifically for web, so it is possible that you as a developer know more than him about programming. Another option could be that he is just trying to hide solutions that would give away the challenges. 
- Rene is open for later conversations about setting it up yourself or development/hacking/pentesting/interesting projects. The ways of reaching him are in the slides. They will be made available after the workshop.
- Bjorn Kimminich deserves a lot of credit for setting up juice-shop, on which the kiwa workshop is based. 
- Other credits go out to the people that helped Rene set up the workshop, including the configuration of containers on the dev environment which you will use for this workshop and just for setting up this workshop in collaboration with Rene

#### [The goal of the workshop] (# goal)
The kiwa workshop contains different challenges with different levels of difficulty, with the aim of uncovering the underlying security problems and exploiting them to solve challenges. These vulnerabilities have been intentionally planted in the application for exactly that purpose, but in a way that also corresponds to "real-life" web development and its vulnerabilities. The Webshop is based on the OWASP Web top 10 list.

Progress is tracked by the application with the help of push notifications for successful exploits and a scoreboard for the progress overview. Finding this scoreboard is one of the (easiest) challenges. The idea behind this workshop is to use gamification techniques to motivate the teams to solve as many challenges as possible, learn new things from the hacker's perspective, and to compete against other teams. Because not everyone is a (web)-developer we try to spread the different expertises per teams.



THE TEXT BELOW IS A SUMMARY OF THE GITBOOK by bkimminich about the juice-shop -> Kiwa workshop.
### Architecture overview

The Kiwa Workshop is a pure web application implemented in JavaScript
and TypeScript (which is compiled into regular JavaScript). In the
frontend the popular [Angular](https://angular.io/) framework is used to
create a so-called _Single Page Application_. The user interface layout
is implementing Google's [Material Design](https://material.io/) using
[Angular Material](https://material.angular.io/) components. It uses
[Angular Flex-Layout](https://github.com/angular/flex-layout) to achieve
responsiveness. All icons found in the UI are originating from the
[Font Awesome](https://fontawesome.com) library.

JavaScript is also used in the backend as the exclusive programming
language: An [Express](http://expressjs.com) application hosted in a
[Node.js](https://nodejs.org) server delivers the client-side code to
the browser. It also provides the necessary backend functionality to the
client via a RESTful API. As an underlying database a light-weight
[SQLite](https://www.sqlite.org) was chosen, because of its file-based
nature. This makes the database easy to create from scratch
programmatically without the need for a dedicated server.
[Sequelize](http://docs.sequelizejs.com) and
[finale-rest](https://www.npmjs.com/package/finale-rest) are used as an
abstraction layer from the database. This allows to use dynamically
created API endpoints for simple interactions (i.e. CRUD operations)
with database resources while still allowing to execute custom SQL for
more complex queries.

As an additional data store a [MarsDB](https://github.com/c58/marsdb) is
part of the Kiwa Workshop. It is a JavaScript derivate of the widely
used [MongoDB](https://www.mongodb.com) NoSQL database and compatible
with most of its query/modify operations.

The push notifications that are shown when a challenge was successfully
hacked, are implemented via
[WebSocket Protocol](https://tools.ietf.org/html/rfc6455). The
application also offers convenient user registration via
[OAuth 2.0](https://oauth.net/2/) so users can sign in with their Google
accounts.

### Browser

When hacking a web application a good internet browser is mandatory. The
emphasis lies on _good_ here, so you do _not_ want to use Internet
Explorer. Other than that it is up to your personal preference. Chrome
and Firefox both work fine from the authors experience.

#### Browser development toolkits

When choosing a browser to work with you want to pick one with good
integrated (or pluggable) developer tooling. Google Chrome and Mozilla
Firefox both come with powerful built-in _DevTools_ which you can open
via the `F12`-key.

When hacking a web application that relies heavily on JavaScript, **it
is essential to your success to monitor the _JavaScript Console_
permanently!** It might leak valuable information to you through error
or debugging logs!

Other useful features of browser DevTools are their network overview as
well as insight into the client-side JavaScript code, cookies and other
local storage being used by the application.

#### Tools for HTTP request tampering

[Tamper Chrome](https://chrome.google.com/webstore/detail/tamper-chrome-extension/hifhgpdkfodlpnlmlnmhchnkepplebkb)
lets you monitor and - more importantly - modify HTTP requests _before_
they are submitted from the browser to the server.

Mozilla Firefox has built-in tampering capabilities and does not need a
plugin. On the _Network_ tab of Firefox's DevTools you have the option
to _Edit and Resend_ every recorded HTTP request.

Tampering is extremely useful when probing for holes in the server-side
validation logic. It can also be helpful when trying to bypass certain
input validation or access restriction mechanisms, that are not properly
checked _on the server_ once more.

An API testing plugin like
[PostMan](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop)
for Chrome allows you to communicate with the RESTful backend of a web
application directly. Skipping the UI can often be useful to circumvent
client-side security mechanisms or simply get certain tasks done faster.
Here you can create requests for all available HTTP verbs (`GET`,
`POST`, `PUT`, `DELETE` etc.) with all kinds of content-types, request
headers etc.

If you feel more at home on the command line, `curl` will do the trick
just as fine as the recommended browser plugins.

#### Scripting tools

A small number of challenges is not realistically solvable manually
unless you are cheating or are incredibly 🍀-lucky.

For these challenges you will require to write some scripts that for
example can submit requests with different parameter values
automatically in a short time. As long as the tool or language of choice
can submit HTTP requests, you should be fine. Use whatever you are most
familiar with.

If you have little experience in programming, best pick a language that
is easy to get into and will give you results without forcing you to
learn a lot of syntax elements or write much _boilerplate code_. Python,
Ruby or JavaScript give you this simplicity and ease-of-use. If you
consider yourself a "command-line hero", Bash or PowerShell will get the
job done for you. Languages like Java, C# or Perl are probably less
suitable for beginners. In the end it depends entirely on your
preferences, but being familiar with at least one programming language
is kind of mandatory if you want to get 100% on the score board.

> In computer programming, boilerplate code or boilerplate refers to
> sections of code that have to be included in many places with little
> or no alteration. It is often used when referring to languages that
> are considered verbose, i.e. the programmer must write a lot of code
> to do minimal jobs.[^1]

### Penetration testing tools

You _can_ solve all challenges just using a browser and the
plugins/tools mentioned above. If you are new to web application hacking
(or penetration testing in general) this is also the _recommended_ set
of tools to start with. In case you have experience with professional
pentesting tools, you are free to use those! And you are _completely
free_ in your choice, so expensive commercial products are just as fine
as open source tools. With this kind of tooling you will have a
competitive advantage for some of the challenges, especially those where
_brute force_ is a viable attack. But there are just as many
multi-staged vulnerabilities in the Kiwa Workshop where - at the time
of this writing - automated tools would probably not help you at all.

In the following sections you find some recommended pentesting tools in
case you want to try one. Please be aware that the tools are not trivial
to learn - let alone master. Trying to learn about the web application
security basics _and_ hacking tools _at the same time_ is unlikely to
get you very far in either of the two topics.

#### Intercepting proxies

An intercepting proxy is a software that is set up as _man in the
middle_ between your browser and the application you want to attack. It
monitors and analyzes all the HTTP traffic and typically lets you
tamper, replay and fuzz HTTP requests in various ways. These tools come
with lots of attack patterns built in and offer active as well as
passive attacks that can be scripted automatically or while you are
surfing the target application.

The open-source
[OWASP Zed Attack Proxy (ZAP)](https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project)
is such a software and offers many useful hacking tools for free:

> ZAP is an easy to use integrated penetration testing tool for finding
> vulnerabilities in web applications. It is designed to be used by
> people with a wide range of security experience and as such is ideal
> for developers and functional testers who are new to penetration
> testing. ZAP provides automated scanners as well as a set of tools
> that allow you to find security vulnerabilities manually.[^2]



#### Pentesting Linux distributions

Instead of installing a tool such as ZAP on your computer, why not take
it, add _several hundred_ of other offensive security tools and put them
all into a ready-to-use Linux distribution? Entering
[Kali Linux](https://www.kali.org) and similar toolboxes:

> Kali Linux is a Debian-based Linux distribution aimed at advanced
> Penetration Testing and Security Auditing. Kali contains several
> hundred tools aimed at various information security tasks, such as
> Penetration Testing, Forensics and Reverse Engineering.[^3]

The keyword in the previous quote is _advanced_! More precisely, Kali
Linux is _easily overwhelming_ when beginners try to work with it, as
even the Kali development team states:

> As the distribution’s developers, you might expect us to recommend
> that everyone should be using Kali Linux. The fact of the matter is,
> however, that Kali is a Linux distribution specifically geared towards
> professional penetration testers and security specialists, and given
> its unique nature, it is __NOT__ a recommended distribution if you’re
> unfamiliar with Linux \[...\]. Even for experienced Linux users, Kali
> can pose some challenges.[^4]

Although there exist some more light-weight pentesting distributions,
they basically still present a high hurdle for people new to the IT
security field. If you still feel up to it, give Kali Linux a try!

### Internet

You are free to use Google during your hacking session to find helpful
websites or tools. The Kiwa Workshop is leaking useful information
all over the place if you know where to look, but sometimes you simply
need to extend your research to the Internet in order to gain some
relevant piece of intel to beat a challenge.

### Source code

Kiwa Workshop is supposed to be attacked in a "black box" manner. That
means you cannot look into the source code to search for
vulnerabilities. As the application tracks your successful attacks on
its challenges, the code must contain checks to verify if you succeeded.
These checks would give many solutions away immediately.

The same goes for several other implementation details, where
vulnerabilities were arbitrarily programmed into the application. These
would be obvious when the source code is reviewed.

Finally the end-to-end test suite of Kiwa Workshop was built hack all
challenges automatically, in order to verify they can all be solved.
These tests deliver all the required attacks on a silver plate when
reviewed.

### GitHub repository

While stated earlier that "the Internet" is fine as a helpful resource,
consider the GitHub repositories https://github.com/bkimminich/juice-shop
and https://github.com/mu71l473d/juice-shop as entirely off limits. 
First and foremost because it contains the source code (see above).

Additionally it hosts the issue tracker of the project, which is used
for idea management and task planning as well as bug tracking. You can
of course submit an issue if you run into technical problems that are
not covered by the [Troubleshooting section of the README.md](). You
just should not read issues labelled `challenge` as they might contain
spoilers or solutions.

Of course you are explicitly allowed to view
[the repository's README.md page](https://github.com/bkimminich/juice-shop/blob/master/README.md),
which contains no spoilers but merely covers project introduction, setup
and troubleshooting. Just do not "dig deeper" than that into the
repository files and folders.

### Database table `Challenges`

The challenges (and their progress) live in one database together with
the rest of the application data, namely in the `Challenges` table. Of
course you could "cheat" by simply editing the state of each challenge
from _unsolved_ to _solved_ by setting the corresponding `solved` column
to `1`. You then just have to keep your fingers crossed, that nobody
ever asks you to _demonstrate how_ you actually solved all the 4- and
5-star challenges so quickly.

### Configuration REST API Endpoint

The Kiwa Workshop offers a URL to retrieve configuration information which
is required by the customization feature that allows
redressing the UI and overwriting the product catalog:
<http://localhost:3000/rest/admin/application-configuration>

The returned JSON contains spoilers for all challenges that depend on a
product from the inventory which might be customized. As not all
customization can be prepared on the server side, exposing this REST
endpoint is unavoidable for the customization feature to work properly.

### Score Board HTML/CSS

In the current context it is suffice to say, that you could manipulate
the score board in the web browser to make challenges _appear as
solved_. Please be aware that this "cheat" is even easier (and more
embarrassing) to uncover in a classroom training than the previously
mentioned database manipulation: A simple reload of the score board URL
will let all your local CSS changes vanish in a blink and reveal your
_real_ hacking progress.

# Vulnerability Categories

The vulnerabilities found in the Kiwa Workshop are categorized into
several different classes. Most of them cover different risk or
vulnerability types from well-known lists or documents, such as
[OWASP Top 10](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
or MITRE's [Common Weakness Enumeration](https://cwe.mitre.org/). The
following table presents a mapping of the Kiwa Workshop's categories to
OWASP and CWE (without claiming to be complete).

### Category Mappings

| Category                    | OWASP                                                                                                                                                                                                                         | CWE                                                                                                                                                                                                                                            |
|:----------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Broken Access Control       | [A5:2017](https://www.owasp.org/index.php/Top_10-2017_A5-Broken_Access_Control)                                                                                                                                               | [CWE-22](https://cwe.mitre.org/data/definitions/22.html), [CWE-285](https://cwe.mitre.org/data/definitions/285.html), [CWE-639](https://cwe.mitre.org/data/definitions/639.html)                                                               |
| Broken Anti-Automation      | [OWASP-AT-004](https://www.owasp.org/index.php/Testing_for_Brute_Force_(OWASP-AT-004)), [OWASP-AT-010](https://www.owasp.org/index.php/Testing_for_Race_Conditions_%28OWASP-AT-010%29)                                        | [CWE-362](http://cwe.mitre.org/data/definitions/362.html)                                                                                                                                                                                      |
| Broken Authentication       | [A2:2017](https://www.owasp.org/index.php/Top_10-2017_A2-Broken_Authentication)                                                                                                                                               | [CWE-287](https://cwe.mitre.org/data/definitions/287.html), [CWE-352](https://cwe.mitre.org/data/definitions/352.html)                                                                                                                         |
| Cross Site Scripting (XSS)  | [A7:2017](https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_%28XSS%29)                                                                                                                                      | [CWE-79](https://cwe.mitre.org/data/definitions/79.html)                                                                                                                                                                                       |
| Cryptographic Issues        | [A3:2017](https://www.owasp.org/index.php/Top_10-2017_A3-Sensitive_Data_Exposure)                                                                                                                                             | [CWE-326](https://cwe.mitre.org/data/definitions/326.html), [CWE-327](https://cwe.mitre.org/data/definitions/327.html), [CWE-328](https://cwe.mitre.org/data/definitions/328.html), [CWE-950](https://cwe.mitre.org/data/definitions/950.html) |
| Improper Input Validation   | [ASVS V5](https://www.owasp.org/index.php/ASVS_V5_Input_validation_and_output_encoding)                                                                                                                                       | [CWE-20](https://cwe.mitre.org/data/definitions/20.html)                                                                                                                                                                                       |
| Injection                   | [A1:2017](https://www.owasp.org/index.php/Top_10-2017_A1-Injection)                                                                                                                                                           | [CWE-74](https://cwe.mitre.org/data/definitions/74.html)                                                                                                                                                                                       |
| Insecure Deserialization    | [A8:2017](https://www.owasp.org/index.php/Top_10-2017_A8-Insecure_Deserialization)                                                                                                                                            | [CWE-502](https://cwe.mitre.org/data/definitions/502.html)                                                                                                                                                                                     |
| Miscellaneous               | -                                                                                                                                                                                                                             | -                                                                                                                                                                                                                                              |
| Security Misconfiguration   | [A6:2017](https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration), [A10:2017](https://www.owasp.org/index.php/Top_10-2017_A10-Insufficient_Logging%26Monitoring)                                            | [CWE-209](https://cwe.mitre.org/data/definitions/209.html)                                                                                                                                                                                     |
| Security through Obscurity  | -                                                                                                                                                                                                                             | [CWE-656](https://cwe.mitre.org/data/definitions/656.html)                                                                                                                                                                                     |
| Sensitive Data Exposure     | [A3:2017](https://www.owasp.org/index.php/Top_10-2017_A3-Sensitive_Data_Exposure), [OTG-CONFIG-004](https://www.owasp.org/index.php/Review_Old,_Backup_and_Unreferenced_Files_for_Sensitive_Information_%28OTG-CONFIG-004%29) | [CWE-200](https://cwe.mitre.org/data/definitions/200.html), [CWE-530](https://cwe.mitre.org/data/definitions/530.html), [CWE-548](https://cwe.mitre.org/data/definitions/548.html)                                                             |
| Unvalidated Redirects       | [A10:2013](https://www.owasp.org/index.php/Top_10_2013-A10-Unvalidated_Redirects_and_Forwards)                                                                                                                                | [CWE-601](https://cwe.mitre.org/data/definitions/601.html)                                                                                                                                                                                     |
| Vulnerable Components       | [A9:2017](https://www.owasp.org/index.php/Top_10-2017_A9-Using_Components_with_Known_Vulnerabilities)                                                                                                                         | [CWE-829](https://cwe.mitre.org/data/definitions/829.html)                                                                                                                                                                                     |
| XML External Entities (XXE) | [A4:2017](https://www.owasp.org/index.php/Top_10-2017_A4-XML_External_Entities_%28XXE%29)                                                                                                                                     | [CWE-611](https://cwe.mitre.org/data/definitions/611.html)                                                                                                                                                                                     |


### Challenge tracking

#### The Score Board

In order to motivate you to hunt for vulnerabilities, it makes sense to
give you at least an idea what challenges are available in the
application. Also you should know when you actually solved a challenge
successfully, so you can move on to another task. Both these cases are
covered by the application's score board.

On the score board you can view a list of all available challenges with
a brief description. Some descriptions are _very explicit_ hacking
instructions. Others are just _vague hints_ that leave it up to you to
find out what needs to be done.

The challenges are rated with a difficulty level between ⭐ and
⭐⭐⭐⭐⭐⭐, with more stars representing a higher difficulty. To make the
list of challenges less daunting, they are clustered by difficulty. By
default only the 1-star challenges are unfolded. You can open or
collapse all challenge blocks as you like. Collapsing a block has _no
impact_ on whether you can _solve_ any of its challenges.

The difficulty ratings have been continually adjusted over time based on
user feedback. The ratings allow you to manage your own hacking pace and
learning curve significantly. When you pick a 5- or 6-star challenge you
should _expect_ a real challenge and should be less frustrated if you
fail on it several times. On the other hand if hacking a 1- or 2-star
challenge takes very long, you might realize quickly that you are on a
wrong track with your chosen hacking approach.

Finally, each challenge states if it is currently _unsolved_ or
_solved_. The current overall progress is represented in a progress bar
on top of the score board. Especially in group hacking sessions this
allows for a bit of competition between the participants.

If not deliberately turned off you can hover over each _unsolved_ label to see a hint for that
challenge. 

#### Challenge Filters

Additional to the filtering by difficulty, you can filter the Score
Board by challenge categories, e.g. to focus your
hacking efforts on specific vulnerabilities. You can also hide all
solved challenges to reduce the level of distraction on the Score Board.

🐌 Selecting _Show all_ for all difficulties and all challenges might
impact the load time of the Score Board significantly!

#### Success notifications

The Kiwa Workshop employs a simple yet powerful gamification
mechanism: Instant success feedback! Whenever you solve a hacking
challenge, a notification is _immediately_ shown on the user interface.

This feature makes it unnecessary to switch back and forth between the
screen you are attacking and the score board to verify if you succeeded.
Some challenges will force you to perform an attack outside of the Workshop
web interface, e.g. by interacting with the REST API directly. In
these cases the success notification will light up when you come back to
the regular web UI the next time.

To make sure you do not miss any notifications they do not disappear
automatically after a timeout. You have to dismiss them explicitly. In
case a number of notifications "piled up" it is not necessary to dismiss
each one individually, as you can simply `Shift`-click one of their
_X_-buttons to dismiss all at the same time.

#### Automatic saving and restoring hacking progress
To keep the resilience against data corruption but allow users to _pick
up where they left off_ after a server restart, your hacking progress is
automatically saved whenever you solve a challenge - as long as you
allow Browser cookies!

After restarting the server, once you visit the application your hacking
progress is automatically restored:

The auto-save mechanism keeps your progress for up to 30 days after your
previous hacking session. When the score board is restored to its prior
state, a torrent of success notifications will light up - depending on
how many challenges you solved up to that point. As mentioned earlier
these can be bulk-dismissed by `Shift`-clicking any of the _X_-buttons.

If you want to start over with a fresh hacking session, simply click the
_Delete cookie to clear hacking progress_ button. After the next server
restart, your score board will be blank.

#### Potentially dangerous challenges

Some challenges can cause potential harm or pose some danger for your
computer, i.e. the XXE, SSTi and Deserialization challenges as well as
two of the NoSQLi challenges. These simply cannot be sandboxed in a 100%
secure way. These are only dangerous if you use actually malicious
payloads, so please do not play with payloads you do not fully
understand.

For safety reasons all potentially dangerous challenges are disabled
(along with their underlying vulnerabilities) in containerized
environments. By default this applies to Docker and Heroku. You can set
`safetyOverride: true` in the configration. Please use at your own risk.

    
    
    
