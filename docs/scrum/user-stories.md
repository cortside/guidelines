# User Story Guidelines

## User stories

A user story is a documented description of a software feature seen from the end-user perspective. The user story describes what exactly the user wants the system to do. In Agile projects, user stories are organized in a backlog, which is an ordered list of product functions. Currently, user stories are considered to be the best format for backlog items.

User stories are often written on index cards or sticky notes, stored in a shoe box, and arranged on walls or tables to facilitate planning and discussion. As such, they strongly shift the focus from writing about features to discussing them. In fact, these discussions are more important than whatever text is written.

A typical user story is written like this:
> *As a &lt;type of user&gt;, I want &lt;some goal&gt; so that &lt;some reason&gt;.*

User stories must be accompanied by acceptance criteria. These are the conditions that the product must satisfy to be accepted by a user, stakeholders, or a product owner. Each user story must have at least one acceptance criterion. Effective acceptance criteria must be testable, concise, and completely understood by all team members and stakeholders. They can be written as checklists, plain text, or by using Given/When/Then format.

> Example:
>
> As a user, I want to use a search field to type a city, name, or street, so that I could find matching hotel options.
>
> *Acceptance*
>
> * Should have a search field is available on the top-bar.
> * Should start search when the user clicks Submit.
> * Should have default placeholder is a grey text Type the name.
> * Should clear placeholder text when the user starts typing.
> * Should use English as search language.
> * Should limit user from typing no more than 200 symbols.
> * Should not support special symbols.  If the user has typed a special symbol in the search input, it displays the warning massage: *Search input cannot contain special symbols.*

All user stories must fit the **INVEST quality model**:

**Independent.** This means that you can schedule and implement each user story separately. This is very helpful if you implement *continuous integration processes*.

**Negotiable.** This means that all parties agree to prioritize negotiations over specification. This also means that details will be created constantly during development.

**Valuable.** A story must be valuable to the customer.  You should ask yourself from the customer’s perspective “why” you need to implement a given feature.

**Estimatable.** A quality user story can be estimated. This will help a team schedule and prioritize the implementation. The bigger the story is, the harder it is to estimate it.

**Small.** Good user stories tend to be small enough to plan for short production releases. Small stories allow for more specific estimates.

**Testable.** If a story can be tested, it’s clear enough and good enough. Tested stories mean that requirements are done and ready for use.

## User story granularity

One of the benefits of agile user stories is that they can be written at varying levels of detail. We can write a user story to cover large amounts of functionality. These large user stories are generally known as epics. Here is an epic agile user story example from a desktop backup product:

* As a user, I can backup my entire hard drive.

Because an epic is generally too large for an agile team to complete in one iteration, it is split into multiple smaller user stories before it is worked on. The epic above could be split into dozens (or possibly hundreds), including these two:

* As a power user, I can specify files or folders to backup based on file size, date created and date modified.
* As a user, I can indicate folders not to backup so that my backup drive isn't filled up with things I don't need saved.

## Adding user story detail

* TODO: how to break up a story

## Who writes user stories?

Anyone can write user stories. It's the product owner's responsibility to make sure a product backlog of agile user stories exists, but that doesn’t mean that the product owner is the one who writes them. Over the course of a good agile project, you should expect to have user story examples written by each team member.

Also, note that who writes a user story is far less important than who is involved in the discussions of it.

## Roles responsible and how acceptance criteria are created

Some of the criteria are defined and written by the product owner when he or she creates the product backlog. And the others can be further specified by the team during user stories discussions after sprint planning. There are no strict recommendations to choosing the person responsible for writing the criteria. The client can document them if he or she has ample technical and product documentation knowledge. In this case, the client negotiates the criteria with the team to avoid mutual misunderstandings. Otherwise the criteria are created by a product owner, business analyst, requirements analyst, or a project manager.
  
## Why is the acceptance criteria important?

Success of any project depends on the ability of a development team to meet their client’s needs. The communication between the client and the development team plays a vital role in delivering a solution that fits product and market requirements. The issues arise if customers explain their needs too vaguely and the team can’t understand clear requirements and eventually the business problem behind them. Imagine that you ask your team to enable users to search for a product in an online bookstore by categories. You expect to have a clear interface with category links to click on them (e.g. fantasy, non-fiction, historic, etc.) After two weeks of development, you receive a search bar feature where users must type in the category they interested in, instead of browsing pre-listed categories. While this also works, your initial goal was to expose all available categories and let users explore further.

## Acceptance criteria main purposes

Clarifying the stakeholder’s requirements is a high-level goal. To make the purposes of AC clearer, let’s break them down.

**Feature scope detalization.** AC define the boundaries of user stories. They provide precise details on functionality that help the team understand whether the story is completed and works as expected.

**Describing negative scenarios.** Yor AC may require the system to recognize unsafe password inputs and prevent a user from proceeding further. Invalid password format is an example of a so-called negative scenario when a user does invalid inputs or behaves unexpectedly. AC define these scenarios and explain how the system must react on them.

**Setting communication.** Acceptance criteria synchronize the visions of the client and the development team. They ensure that everyone has a common understanding of the requirements: Developers know exactly what kind of behavior the feature must demonstrate, while stakeholders and the client understand what’s expected from the feature.

**Streamlining acceptance testing.** AC are the basis of the user story acceptance testing. Each acceptance criterion must be independently testable and thus have a clear pass or fail scenarios. They can also be used to verify the story via automated tests.

**Feature estimation.** Acceptance criteria specify what exactly must be developed by the team. Once the team has precise requirements, they can split user stories into tasks that can be correctly estimated.

## Acceptance criteria types and structures

AC can be written in different formats. There are two most common ones, and the third option is to devise your own format:

* scenario-oriented (Given/When/Then)
  * Scenario-oriented format of writing AC is known as the Given/When/Then (GWT) type.
    * *Given* some precondition
    * *When* I do some action
    * *Then* I expect some result
    * *And* – used to continue any of the statements
  * In some cases, it’s difficult to fit acceptance criteria into the Given/When/Then structure. For instance, GWT would hardly be useful for the following cases:
    * You’re working with user stories that describe the system level functionality that needs other methods of quality assurance.
    * The target audience for acceptance criteria doesn’t need precise details of the test scenarios.
    * GWT scenarios don’t fit to describing design and user experience constraints of a feature. Developers may miss a number of critical details.
* rule-oriented (checklist)
  * The rule-oriented form entails that there is a set of rules that describe the behavior of a system. Based on these rules, you can draw specific scenarios. 
  * Usually, criteria composed using this form look like a simple bullet list
* custom formats
  * Most user stories can be covered with two formats mentioned above. However, you can invent your own acceptance criteria given they serve their purposes, are written clearly in plain English, and can’t be misinterpreted. Some teams even use plain text.

## 7 Tips for Writing Acceptance Criteria:
* Each product backlog item or user story should have at least one acceptance criteria. Hey, don’t take writing acceptance criteria lightly or think of skipping it.
* Acceptance Criteria is written before implementation – this is obvious yet frequently missed by teams. Write acceptance criteria after the implementation and miss the benefits.
* Each acceptance criterion is independently testable. Why shouldn’t it be?
* Acceptance criteria must have a clear Pass / Fail result. Write complex and long sentences at your own risk.
* It focuses on the end result – What. Not the solution approach – How.
* Include functional as well as non-functional criteria – when relevant.
* Team members write acceptance criteria and the Product Owner verifies it. It confirms the PO and the team have shared understanding of the user story.

## Examples

scenario-oriented (Given/When/Then)
> As a user, I want to be able to request the cash from my account in ATM so that I will be able to receive the money from my account quickly and in different places.
>
> Acceptance criteria 1:
>
> Given: that the account is creditworthy
> And: the card is valid
> And: the dispenser contains cash
> When: the customer requests the cash
> Then: ensure the account is debited
> And: ensure cash is dispensed
> And: ensure the card is returned
>
> Acceptance criteria 2:
>
> Given: that the account is overdrawn
> And: the card is valid
> When: the customer requests the cash
> Then: ensure the rejection message is displayed
> And: ensure cash isn’t dispensed

rule-oriented (checklist)
> As a user, I want to use a search field to type a city, name, or street, so that I could find matching hotel options.
>
> *Acceptance*
>
> * The search field is placed on the top bar
> * Search starts once the user clicks “Search”
> * The field contains a placeholder with a grey-colored text: “Where are you going?”
> * The placeholder disappears once the user starts typing
> * Search is performed if a user types in a city, hotel name, street, or all combined
> * Search is in English, French, German, and Ukrainian
> * The user can’t type more than 200 symbols
> * The search doesn’t support special symbols (characters). If the user has typed a special symbol, show the warning message: “Search input cannot contain  special symbols.”

custom format (MDW-974)

# Populate table ApplicationSource in oa datamart
​
PartnerPortal needs application data aggregated into table ApplicationSource.
​
## Tables & Population
​
### ApplicationSource
​
| Column        | Type                   | Notes                                                                                    |     |     |     |
| ------------- | ---------------------- | ---------------------------------------------------------------------------------------- | --- | --- | --- |
| Date          | date, not null         |                                                                                          |     |     |     |
| SponsorId     | int, not null          | Foreign key to Sponsor table SponsorId column                                            |     |     |     |
| ContractorId  | int, not null          | Foreign key to Contractor table ContractorId column                                      |     |     |     |
| SalespersonId | int, null              | Foreign key to Salesperson table SalespersonId column                                    |     |     |     |
| Source        | nvarchar(25), not null | Possible values are Online, OnlineToPhone, Mobile, MobileToPhone, Phone, Api, ApiToPhone |     |     |     |
| Count         | int, not null          |                                                                                          |     |     |     |
| Dollars       | money, not null        |                                                                                          |     |     |     |
​
Please see notes.txt for Andrew’s notes on Source definition.
​
### Sponsor
​
| Column        | Type                    | Notes                |     |     |     |
| ------------- | ----------------------- | -------------------- | --- | --- | --- |
| SponsorId     | int, not null           | Primary key of table |     |     |     |
| SponsorNumber | int, not null           |                      |     |     |     |
| Name          | nvarchar(100), not null |                      |     |     |     |
​
Data of this table comes from OA table Sponsor.
​
### Contractor
​
| Column           | Type                    | Notes                |     |     |     |
| ---------------- | ----------------------- | -------------------- | --- | --- | --- |
| ContractorId     | int, not null           | Primary key of table |     |     |     |
| ContractorNumber | int, not null           |                      |     |     |     |
| Name             | nvarchar(100), not null |                      |     |     |     |
​
Data of this table comes from OA table Contractor.
​
### Salesperson
​
| Column        | Type               | Notes                                               |     |     |     |
| ------------- | ------------------ | --------------------------------------------------- | --- | --- | --- |
| SalespersonId | int, not null      | Primary key of table                                |     |     |     |
| ContractorId  | int, not null      | Foreign key to Contractor table ContractorId column |     |     |     |
| Name          | nvarchar(60), null |
​
Data of this table comes from OA table Salesperson.

## Blatently plagarized sources
* https://www.agileconnection.com/article/user-story-heuristics-understanding-agile-requirements#:~:text=People%20call%20user%20stories%20a,for%20work%20to%20be%20done.
* https://www.mountaingoatsoftware.com/agile/user-stories#:~:text=What%20is%20a%20user%20story,so%20that%20.
* https://www.altexsoft.com/blog/business/functional-and-non-functional-requirements-specification-and-types/
altexsoft.com/blog/business/acceptance-criteria-purposes-formats-and-best-practices/
* https://agileforgrowth.com/blog/acceptance-criteria-checklist/
