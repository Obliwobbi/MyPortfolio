+++
date = '2026-02-08'
draft = false
title = 'How I get started!'
tags = ["how-to", "planning", "JPA", "java", "JPQL"]
author = 'Toby Alexander West Mietke Hartzberg'
+++

# Starting simple

So, putting the cards on the table: I have a hard time getting started with… well, everything.
I have always had that issue. But that’s not what I’m really going to talk about today… or write about, he he, I’ll have to get used to this blogging thing.

What I do want to write about today is how I get started, and this is something my dear wife has taught me over time:

*Start with something simple.*

This sounds obvious, almost trivial, but in practice, it is incredibly hard when you are building software and constantly thinking ahead.

To help myself get unstick, I decided to follow **four very simple steps**, which I'll walk through there.

### 1. Write one sentence

Before writing any code, drawing diagrams, or thinking about architecture, I forced myself to write one single sentence, describing what I wanted to achieve.

And the sentence I wrote:<br>
>*I want to be able to save a user/member in a database and fetch it again.*

With that sentence, I knew what was needed, a user entity and a database, easy! Now to what that entity requires, so on to step two.

### 2. What is the least that needs to exist?

Here, I try to answer three simple questions in my sentence:

**What *thing* am I saving?** <br>
-- *a user*<br>
**What does it HAS to have?** <br>
-- *id,email*<br>
**Where is it saved?** <br> 
-- *a database (JPA)*

Awesome, super simple, almost boring, I know. But for some people, this is the way to go!

### 3. Make it boring!

This is almost the most understimated step, but making it so simple its boring is how most people can better get through. If you keep it at:
* One entity
* One table
* One repository/DAO

If this feels like its *too* simple, thats a good sign! Keep going. The goal here is not to impress **anyone**, this is for you to get started!

### 4. Only expand after something works

Only after I could:
* Persist a *user*
* Retrieve it again
* See an ID printed in the console (that matched)

then I allowed myself to think about what comes next.

This mindset shift alone helped me move forward instead of getting stuck to planning a system that didnt exist yet.

# First domain model

The first entity I implemented was ```User```, containing only essential fields:
* id
* email
* firstname
* lastname
* role (as enum)
* passwordHash (placeholder for later security work)

I also made a few conscous early decisions:
* Database constraints (nullable, unique)
* Enums instead of strings
* Letting the database help enforce rules

This keeps business rules close to the data model and avoids unnecessary complexity later.

# Introducing Company and relationships

Once the basic ```User``` persistence worked, I introduced a ```Company``` entity.

A user must belong to a company in order for a membership to make sense later, so instead of storing a raw companyId, I modeled this as a JPA **Many-to-One** relationship.

This reflects the real-world domain better and allows Hibernate to manage:
* Foreign keys
* Joins
* Object navigation

# Core domain - Week 1
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" contentStyleType="text/css" data-diagram-type="CLASS" height="477px" preserveAspectRatio="none" style="width:538px;height:577px;background:#FFFFFF;" version="1.1" viewBox="0 0 338 377" width="438px" zoomAndPan="magnify"><title>Core Domain - Week 1</title><defs/><g><g class="title" data-source-line="1"><text fill="#000000" font-family="sans-serif" font-size="14" font-weight="bold" lengthAdjust="spacing" textLength="147.8135" x="88.1846" y="25.1074">Core Domain - Week 1</text></g><!--class User--><g class="entity" data-entity="User" data-source-line="3" data-uid="ent0002" id="entity_User"><rect fill="#F1F1F1" height="159.7266" rx="2.5" ry="2.5" style="stroke:#181818;stroke-width:0.5;" width="152.8408" x="7" y="46.6211"/><ellipse cx="64.3911" cy="62.6211" fill="#ADD1B2" rx="11" ry="11" style="stroke:#181818;stroke-width:1;"/><path d="M67.1567,58.4961 Q67.313,58.2773 67.5005,58.168 Q67.688,58.0586 67.9067,58.0586 Q68.2817,58.0586 68.5161,58.3242 Q68.7505,58.5742 68.7505,59.1836 L68.7505,60.6367 Q68.7505,61.2461 68.5161,61.5117 Q68.2817,61.7773 67.9067,61.7773 Q67.563,61.7773 67.3599,61.5742 Q67.1567,61.3867 67.0474,60.8711 Q67.0005,60.5117 66.813,60.3242 Q66.4849,59.9492 65.8755,59.7305 Q65.2661,59.5117 64.6411,59.5117 Q63.8755,59.5117 63.2349,59.8398 Q62.6099,60.168 62.1099,60.918 Q61.6255,61.668 61.6255,62.6992 L61.6255,63.793 Q61.6255,65.0273 62.5161,65.8555 Q63.4067,66.668 65.0005,66.668 Q65.938,66.668 66.5942,66.418 Q66.9849,66.2617 67.4067,65.8242 Q67.6724,65.5586 67.813,65.4805 Q67.9692,65.4023 68.1724,65.4023 Q68.5005,65.4023 68.7505,65.668 Q69.0161,65.918 69.0161,66.2617 Q69.0161,66.6055 68.6724,67.0117 Q68.1724,67.5898 67.3755,67.918 Q66.2974,68.3711 65.0005,68.3711 Q63.4849,68.3711 62.2817,67.7461 Q61.2974,67.2461 60.6099,66.1836 Q59.9224,65.1055 59.9224,63.8242 L59.9224,62.668 Q59.9224,61.3398 60.5317,60.1992 Q61.1567,59.043 62.2505,58.4336 Q63.3442,57.8086 64.5786,57.8086 Q65.313,57.8086 65.9536,57.9805 Q66.6099,58.1367 67.1567,58.4961 Z " fill="#000000"/><text fill="#000000" font-family="sans-serif" font-size="14" lengthAdjust="spacing" textLength="29.5586" x="84.8911" y="68.418">User</text><line style="stroke:#181818;stroke-width:0.5;" x1="8" x2="158.8408" y1="78.6211" y2="78.6211"/><text fill="#000000" font-family="sans-serif" font-size="14" lengthAdjust="spacing" textLength="53.71" x="13" y="97.7285">id : Long</text><text fill="#000000" font-family="sans-serif" font-size="14" lengthAdjust="spacing" textLength="81.6963" x="13" y="116.3496">email : String</text><text fill="#000000" font-family="sans-serif" font-size="14" lengthAdjust="spacing" textLength="105.8135" x="13" y="134.9707">firstname : String</text><text fill="#000000" font-family="sans-serif" font-size="14" lengthAdjust="spacing" textLength="105.0479" x="13" y="153.5918">lastname : String</text><text fill="#000000" font-family="sans-serif" font-size="14" lengthAdjust="spacing" textLength="140.8408" x="13" y="172.2129">passwordHash : String</text><text fill="#000000" font-family="sans-serif" font-size="14" lengthAdjust="spacing" textLength="63.8066" x="13" y="190.834">role : Role</text><line style="stroke:#181818;stroke-width:0.5;" x1="8" x2="158.8408" y1="198.3477" y2="198.3477"/></g><!--class Company--><g class="entity" data-entity="Company" data-source-line="12" data-uid="ent0003" id="entity_Company"><rect fill="#F1F1F1" height="85.2422" rx="2.5" ry="2.5" style="stroke:#181818;stroke-width:0.5;" width="95.2617" x="36" y="285.6211"/><ellipse cx="52.5051" cy="301.6211" fill="#ADD1B2" rx="11" ry="11" style="stroke:#181818;stroke-width:1;"/><path d="M55.2708,297.4961 Q55.427,297.2773 55.6145,297.168 Q55.802,297.0586 56.0208,297.0586 Q56.3958,297.0586 56.6301,297.3242 Q56.8645,297.5742 56.8645,298.1836 L56.8645,299.6367 Q56.8645,300.2461 56.6301,300.5117 Q56.3958,300.7773 56.0208,300.7773 Q55.677,300.7773 55.4739,300.5742 Q55.2708,300.3867 55.1614,299.8711 Q55.1145,299.5117 54.927,299.3242 Q54.5989,298.9492 53.9895,298.7305 Q53.3801,298.5117 52.7551,298.5117 Q51.9895,298.5117 51.3489,298.8398 Q50.7239,299.168 50.2239,299.918 Q49.7395,300.668 49.7395,301.6992 L49.7395,302.793 Q49.7395,304.0273 50.6301,304.8555 Q51.5208,305.668 53.1145,305.668 Q54.052,305.668 54.7083,305.418 Q55.0989,305.2617 55.5208,304.8242 Q55.7864,304.5586 55.927,304.4805 Q56.0833,304.4023 56.2864,304.4023 Q56.6145,304.4023 56.8645,304.668 Q57.1301,304.918 57.1301,305.2617 Q57.1301,305.6055 56.7864,306.0117 Q56.2864,306.5898 55.4895,306.918 Q54.4114,307.3711 53.1145,307.3711 Q51.5989,307.3711 50.3958,306.7461 Q49.4114,306.2461 48.7239,305.1836 Q48.0364,304.1055 48.0364,302.8242 L48.0364,301.668 Q48.0364,300.3398 48.6458,299.1992 Q49.2708,298.043 50.3645,297.4336 Q51.4583,296.8086 52.6926,296.8086 Q53.427,296.8086 54.0676,296.9805 Q54.7239,297.1367 55.2708,297.4961 Z " fill="#000000"/><text fill="#000000" font-family="sans-serif" font-size="14" lengthAdjust="spacing" textLength="59.917" x="66.8396" y="307.418">Company</text><line style="stroke:#181818;stroke-width:0.5;" x1="37" x2="130.2617" y1="317.6211" y2="317.6211"/><text fill="#000000" font-family="sans-serif" font-size="14" lengthAdjust="spacing" textLength="53.71" x="42" y="336.7285">id : Long</text><text fill="#000000" font-family="sans-serif" font-size="14" lengthAdjust="spacing" textLength="83.2617" x="42" y="355.3496">name : String</text><line style="stroke:#181818;stroke-width:0.5;" x1="37" x2="130.2617" y1="362.8633" y2="362.8633"/></g><!--class Role--><g class="entity" data-entity="Role" data-source-line="17" data-uid="ent0004" id="entity_Role"><rect fill="#F1F1F1" height="103.8633" rx="2.5" ry="2.5" style="stroke:#181818;stroke-width:0.5;" width="135.6826" x="195.5" y="74.6211"/><ellipse cx="244.6948" cy="90.6211" fill="#EB937F" rx="11" ry="11" style="stroke:#181818;stroke-width:1;"/><path d="M243.5542,91.418 L243.5542,93.918 L247.8823,93.918 L247.8823,92.9961 Q247.8823,92.3867 248.1167,92.1211 Q248.3667,91.8555 248.7417,91.8555 Q249.1167,91.8555 249.3511,92.1211 Q249.5854,92.3867 249.5854,92.9961 L249.5854,95.6211 L241.5854,95.6211 Q240.9604,95.6211 240.6948,95.3867 Q240.4448,95.1523 240.4448,94.7617 Q240.4448,94.3867 240.7104,94.1523 Q240.9761,93.918 241.5854,93.918 L241.8511,93.918 L241.8511,87.2617 L241.5854,87.2617 Q240.9604,87.2617 240.6948,87.0273 Q240.4448,86.793 240.4448,86.4023 Q240.4448,86.0273 240.6948,85.793 Q240.9604,85.5586 241.5854,85.5586 L249.2104,85.5586 L249.2104,88.1523 Q249.2104,88.7617 248.9761,89.0273 Q248.7573,89.2773 248.3667,89.2773 Q247.9917,89.2773 247.7573,89.0273 Q247.5229,88.7617 247.5229,88.1523 L247.5229,87.2617 L243.5542,87.2617 L243.5542,89.7148 L245.0386,89.7148 Q245.0386,89.0586 245.1636,88.8711 Q245.4292,88.4648 245.8979,88.4648 Q246.2729,88.4648 246.5073,88.7305 Q246.7417,88.9805 246.7417,89.5898 L246.7417,91.5586 Q246.7417,92.1055 246.6167,92.293 Q246.3511,92.6836 245.8979,92.6836 Q245.4292,92.6836 245.1636,92.2773 Q245.0386,92.0898 245.0386,91.418 L243.5542,91.418 Z " fill="#000000"/><text fill="#000000" font-family="sans-serif" font-size="14" lengthAdjust="spacing" textLength="28.793" x="265.1948" y="96.418">Role</text><line style="stroke:#181818;stroke-width:0.5;" x1="196.5" x2="330.1826" y1="106.6211" y2="106.6211"/><text fill="#000000" font-family="sans-serif" font-size="14" lengthAdjust="spacing" textLength="61.4482" x="201.5" y="125.7285">MEMBER</text><text fill="#000000" font-family="sans-serif" font-size="14" lengthAdjust="spacing" textLength="123.6826" x="201.5" y="144.3496">COMPANY_ADMIN</text><text fill="#000000" font-family="sans-serif" font-size="14" lengthAdjust="spacing" textLength="110.4619" x="201.5" y="162.9707">SYSTEM_ADMIN</text><line style="stroke:#181818;stroke-width:0.5;" x1="196.5" x2="330.1826" y1="170.4844" y2="170.4844"/></g><!--link User to Company--><g class="link" data-entity-1="User" data-entity-2="Company" data-source-line="23" data-uid="lnk5" id="link_User_Company"><path codeLine="23" d="M83.5,206.7511 C83.5,233.6211 83.5,256.5811 83.5,279.3611" fill="none" id="User-to-Company" style="stroke:#181818;stroke-width:1;"/><polygon fill="#181818" points="83.5,285.3611,87.5,276.3611,83.5,280.3611,79.5,276.3611,83.5,285.3611" style="stroke:#181818;stroke-width:1;"/><text fill="#000000" font-family="sans-serif" font-size="13" lengthAdjust="spacing" textLength="59.9917" x="84.5" y="251.6494">belongs to</text><text fill="#000000" font-family="sans-serif" font-size="13" lengthAdjust="spacing" textLength="31.7891" x="51.2087" y="228.4203">many</text><text fill="#000000" font-family="sans-serif" font-size="13" lengthAdjust="spacing" textLength="21.6899" x="60.8607" y="274.2237">one</text></g><!--SRC=[PO_D2i8m48JlVOgbznwyzY3Kjg1WghIAz2HHLmtcfoGH4V7TNODAfQVT6MRsOxtm4Y4t5g4mYWiD2MmGBp2Aehtapi7QeOL7120EaCBSw1FjI559il5M1ECehFEQA-oRr1zu7Tsy6NkOSdVk-zR8Twwc4Js_xDoeZklx0Fz_bEAehofqGvILR5BMjEGBjiogTfiLR5QHRjfcLRAFf5huBLQ4Y259nX0RZV6Fci8E9G4TUFC2]--></g></svg>

# Persistence layer - DAO responsibility

For pesistence, I implemented a DAO for each entity.

The DAO's responsibility is *only*:
* Opening an EntityManager
* Handling transactions
* Persisting and retrieving entities

It contains **no business logic**, **no validation** and **no application rules**.

This separation keeps the code easier to reason about and easier to extend later.

# DAO interaction overview
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" contentStyleType="text/css" data-diagram-type="SEQUENCE" height="240px" preserveAspectRatio="none" style="width:565px;height:276px;background:#FFFFFF;" version="1.1" viewBox="0 0 491 240" width="491px" zoomAndPan="magnify"><title>DAO Interaction</title><defs/><g><text fill="#000000" font-family="sans-serif" font-size="14" font-weight="bold" lengthAdjust="spacing" textLength="106.5654" x="191.6282" y="30.1074">DAO Interaction</text><g><title>Main</title><rect fill="#000000" fill-opacity="0.00000" height="124.582" width="8" x="23.1724" y="78.2422"/><line style="stroke:#181818;stroke-width:0.5;stroke-dasharray:5,5;" x1="27" x2="27" y1="78.2422" y2="202.8242"/></g><g><title>CompanyDAO</title><rect fill="#000000" fill-opacity="0.00000" height="124.582" width="8" x="147.5986" y="78.2422"/><line style="stroke:#181818;stroke-width:0.5;stroke-dasharray:5,5;" x1="151.4712" x2="151.4712" y1="78.2422" y2="202.8242"/></g><g><title>UserDAO</title><rect fill="#000000" fill-opacity="0.00000" height="124.582" width="8" x="246.6743" y="78.2422"/><line style="stroke:#181818;stroke-width:0.5;stroke-dasharray:5,5;" x1="249.7261" x2="249.7261" y1="78.2422" y2="202.8242"/></g><g><title>EntityManager</title><rect fill="#000000" fill-opacity="0.00000" height="124.582" width="8" x="345.7568" y="78.2422"/><line style="stroke:#181818;stroke-width:0.5;stroke-dasharray:5,5;" x1="349.6226" x2="349.6226" y1="78.2422" y2="202.8242"/></g><g><title>Database</title><rect fill="#000000" fill-opacity="0.00000" height="124.582" width="8" x="444.8564" y="78.2422"/><line style="stroke:#181818;stroke-width:0.5;stroke-dasharray:5,5;" x1="447.8911" x2="447.8911" y1="78.2422" y2="202.8242"/></g><g class="participant participant-head" data-participant="Main"><rect fill="#E2E2F0" height="32.6211" rx="2.5" ry="2.5" style="stroke:#181818;stroke-width:0.5;" width="44.3447" x="5" y="44.6211"/><text fill="#000000" font-family="sans-serif" font-size="14" lengthAdjust="spacing" textLength="30.3447" x="12" y="66.7285">Main</text></g><g class="participant participant-tail" data-participant="Main"><rect fill="#E2E2F0" height="32.6211" rx="2.5" ry="2.5" style="stroke:#181818;stroke-width:0.5;" width="44.3447" x="5" y="201.8242"/><text fill="#000000" font-family="sans-serif" font-size="14" lengthAdjust="spacing" textLength="30.3447" x="12" y="223.9316">Main</text></g><g class="participant participant-head" data-participant="CompanyDAO"><rect fill="#E2E2F0" height="32.6211" rx="2.5" ry="2.5" style="stroke:#181818;stroke-width:0.5;" width="104.2549" x="99.4712" y="44.6211"/><text fill="#000000" font-family="sans-serif" font-size="14" lengthAdjust="spacing" textLength="90.2549" x="106.4712" y="66.7285">CompanyDAO</text></g><g class="participant participant-tail" data-participant="CompanyDAO"><rect fill="#E2E2F0" height="32.6211" rx="2.5" ry="2.5" style="stroke:#181818;stroke-width:0.5;" width="104.2549" x="99.4712" y="201.8242"/><text fill="#000000" font-family="sans-serif" font-size="14" lengthAdjust="spacing" textLength="90.2549" x="106.4712" y="223.9316">CompanyDAO</text></g><g class="participant participant-head" data-participant="UserDAO"><rect fill="#E2E2F0" height="32.6211" rx="2.5" ry="2.5" style="stroke:#181818;stroke-width:0.5;" width="73.8965" x="213.7261" y="44.6211"/><text fill="#000000" font-family="sans-serif" font-size="14" lengthAdjust="spacing" textLength="59.8965" x="220.7261" y="66.7285">UserDAO</text></g><g class="participant participant-tail" data-participant="UserDAO"><rect fill="#E2E2F0" height="32.6211" rx="2.5" ry="2.5" style="stroke:#181818;stroke-width:0.5;" width="73.8965" x="213.7261" y="201.8242"/><text fill="#000000" font-family="sans-serif" font-size="14" lengthAdjust="spacing" textLength="59.8965" x="220.7261" y="223.9316">UserDAO</text></g><g class="participant participant-head" data-participant="EntityManager"><rect fill="#E2E2F0" height="32.6211" rx="2.5" ry="2.5" style="stroke:#181818;stroke-width:0.5;" width="104.2686" x="297.6226" y="44.6211"/><text fill="#000000" font-family="sans-serif" font-size="14" lengthAdjust="spacing" textLength="90.2686" x="304.6226" y="66.7285">EntityManager</text></g><g class="participant participant-tail" data-participant="EntityManager"><rect fill="#E2E2F0" height="32.6211" rx="2.5" ry="2.5" style="stroke:#181818;stroke-width:0.5;" width="104.2686" x="297.6226" y="201.8242"/><text fill="#000000" font-family="sans-serif" font-size="14" lengthAdjust="spacing" textLength="90.2686" x="304.6226" y="223.9316">EntityManager</text></g><g class="participant participant-head" data-participant="Database"><rect fill="#E2E2F0" height="32.6211" rx="2.5" ry="2.5" style="stroke:#181818;stroke-width:0.5;" width="73.9307" x="411.8911" y="44.6211"/><text fill="#000000" font-family="sans-serif" font-size="14" lengthAdjust="spacing" textLength="59.9307" x="418.8911" y="66.7285">Database</text></g><g class="participant participant-tail" data-participant="Database"><rect fill="#E2E2F0" height="32.6211" rx="2.5" ry="2.5" style="stroke:#181818;stroke-width:0.5;" width="73.9307" x="411.8911" y="201.8242"/><text fill="#000000" font-family="sans-serif" font-size="14" lengthAdjust="spacing" textLength="59.9307" x="418.8911" y="223.9316">Database</text></g><g class="message" data-participant-1="Main" data-participant-2="CompanyDAO"><polygon fill="#181818" points="139.5986,107.5332,149.5986,111.5332,139.5986,115.5332,143.5986,111.5332" style="stroke:#181818;stroke-width:1;"/><line style="stroke:#181818;stroke-width:1;stroke-dasharray:2,2;" x1="27.1724" x2="145.5986" y1="111.5332" y2="111.5332"/><text fill="#000000" font-family="sans-serif" font-size="13" lengthAdjust="spacing" textLength="100.4263" x="34.1724" y="106.2705">createCompany()</text></g><g class="message" data-participant-1="Main" data-participant-2="UserDAO"><polygon fill="#181818" points="238.6743,138.8242,248.6743,142.8242,238.6743,146.8242,242.6743,142.8242" style="stroke:#181818;stroke-width:1;"/><line style="stroke:#181818;stroke-width:1;stroke-dasharray:2,2;" x1="27.1724" x2="244.6743" y1="142.8242" y2="142.8242"/><text fill="#000000" font-family="sans-serif" font-size="13" lengthAdjust="spacing" textLength="72.2363" x="34.1724" y="137.5615">createUser()</text></g><g class="message" data-participant-1="UserDAO" data-participant-2="EntityManager"><polygon fill="#181818" points="337.7568,152.8242,347.7568,156.8242,337.7568,160.8242,341.7568,156.8242" style="stroke:#181818;stroke-width:1;"/><line style="stroke:#181818;stroke-width:1;stroke-dasharray:2,2;" x1="250.6743" x2="343.7568" y1="156.8242" y2="156.8242"/></g><g class="message" data-participant-1="CompanyDAO" data-participant-2="EntityManager"><polygon fill="#181818" points="337.7568,166.8242,347.7568,170.8242,337.7568,174.8242,341.7568,170.8242" style="stroke:#181818;stroke-width:1;"/><line style="stroke:#181818;stroke-width:1;stroke-dasharray:2,2;" x1="151.5986" x2="343.7568" y1="170.8242" y2="170.8242"/></g><g class="message" data-participant-1="EntityManager" data-participant-2="Database"><polygon fill="#181818" points="436.8564,180.8242,446.8564,184.8242,436.8564,188.8242,440.8564,184.8242" style="stroke:#181818;stroke-width:1;"/><line style="stroke:#181818;stroke-width:1;stroke-dasharray:2,2;" x1="349.7568" x2="442.8564" y1="184.8242" y2="184.8242"/></g><!--SRC=[AyaioKbLS77qL_3CAqajIamkoSpFu-BoJSpCKz3LjLDmpiyjICoh12cmKaWkIaqiIOKAQMWYL8KMfnOXAm7nWV9SC76G6jUyaioIIj_4lCJqr28k97Cn9RbGk605BeabYKc9nQa0]--></g></svg>

# Understanding entity state

One of the most important things I learned this week was how **entity state** works in JPA.

When a ```Company``` is persisted in one transaction and later used when creating a ```User```, the entity becomes *detached*..<br>
Hibernate idtentifies entities only by their primary key, so calling ```merge(company)``` ensures the entity is reattached to the current persistence context before persisting the ```User```.

This was a key "aha" moment for me, that clarified how Hibernate knows which database row an object represents.

# Querying data - finding a user by email

Since ```EntityManager.find()``` only works with primary keys, I used JPQL to find users by email:
```java
    SELECT u FROM User u WHERE u.email = :email
```

The DAO then returns an ```Optional<User>```, making the absence of data explicit and avoiding *null*

Example usage:
```java
    userDAO.findByEmail("mail@mail.dk")
        .ifPresentOrElse(
            userFound -> System.out.println(
                "Found: " + userFound.getEmail() + " id=" + userFound.getId()
            ),
            () -> System.out.println("No user found")
        );
```

# Reflection

This session taught me that:
* Getting started is often harder than solving the problem istelf
* Over-planning can block progress
* A small, working foundation beats a large unfinished design
* JPA concepts make much more sense once you see them fail and then work

Most importantly, I now have a **stable base* I can confidently build on in the coming weeks.

# My next steps

* Add more read operations (```findById```,```findAll```, etc.. ALL THE CRUDS!)
* Introduce a service layer when business rules appear
* Prepare for REST endpoints
* Continue expanding the system incrementally

Until next time! See you around!