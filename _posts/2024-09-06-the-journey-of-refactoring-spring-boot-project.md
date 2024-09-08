   ---
   title: The Journey of Refactoring: Lesson Learned in refactoring Java Spring Boot codebase
   created: '2024-09-05T06:51:36.809Z'
   modified: '2021-09-06T04:17:06.115Z'
   categories: [Programming, IT, coding, Java]
   tags: [coding, IT]
   date: '2024-09-06'
   ---


## Intro
Hi! I'm excited to finally kick off my blog and share my experiences with you. The promise of the Spring Boot framework—reducing development time and increasing productivity—has made it a go-to choice for many Java web developers. So, why did I face such a tough time delivering requirements with this framework? Imagine this: a simple JSON field update took me three days to complete, when it should have been done in less than a day!

Taking over a chaotic, unorganized, and poorly managed Spring Boot project was no easy task. In this post, I’ll share my journey of refactoring a project that suffered from rushed timelines, a lack of expertise, and zero documentation. It came with its share of challenges, but I’ll walk you through the key pain points and how I tackled them.

## Why the Codebase Was a Mess!!
Let me be clear: I’m not claiming to be a perfect coder, but this codebase was seriously bad. Below are just some of the reasons why it was such a nightmare.

### 1. 300 lines of SQL Query

![Desktop View](/assets/refactor-blog/sql-query-300.jpg)
_300 lines of SQL Query Repository Class_

Imagine trying to fix a bug in a 300-line SQL query, with no one around to explain what the query even does. This is how I spent two days of my life, and I still couldn't fully understand it (LOL). The project used Spring JPA with Hibernate, but almost every query was a native SQL query embedded with the @Query annotation—completely ignoring the benefits of Hibernate.

![Desktop View](/assets/refactor-blog/sql-query.png)
_Another long SQL Query_

And it wasn’t just one or two queries; about 35% of the queries were over 100 lines long. Instead of writing monstrous queries like this, I refactored the code to use Java Streams. Data transformation and business logic are now handled in the codebase, reducing most queries to under 10 lines—just enough to fetch the data and apply basic filters like dates. I believe this approach is better performance-wise since it manipulates data at the memory level rather than overloading the database (feel free to share your thoughts!).

### 2. 1.2k lines in a single method
No, you didn’t misread that. This project had methods with over 1,000 lines of code. How are you supposed to understand what such a method does? For example, does the recoverMissingTransactions method really need 1.3k lines of code?

![Desktop View](/assets/refactor-blog/1k-lines-code.jpg)

While there’s no strict rule about how many lines a method should have, I usually aim for around 100 lines. Any longer and you’re scrolling through endless lines, which makes it harder to read and maintain the code. If a method exceeds that limit, it’s probably doing too many things and needs to be broken up into smaller methods.

### 3. Improper variable name
Check this out: multiple JsonObject variables with cryptic names. How am I supposed to know which one does what? Now combine this with the fact that many methods were over 1,000 lines long, and you can see why I was pulling my hair out.

![Desktop View](/assets/refactor-blog/bad-variable.png)

Perhaps can I even know what does gg used for ? for sure it's not "good game" xd. Maybe you need to guess first what is it used for, that is the game...

![Desktop View](/assets/refactor-blog/bad-variable-gg.png)

There’s a popular quote: “The hardest part of programming is naming things.” It might be a joke, but it’s true. Good variable names give other developers (and future you) a general understanding of what the variable represents without needing to dig into the surrounding code.

### 4. Lack of DTO, everything is "STRING"
Why use Data Transfer Objects (DTOs) when you can just return everything as a String? This seems to have been the mentality of the original developers. Sure, you can return strings, but this invites a host of issues: hardcoding JSON keys, making it harder for the IDE to assist, and generally slowing down development.

![Desktop View](/assets/refactor-blog/string-function.png)

Instead of transforming data into strings manually, use DTOs. Spring Boot can handle most of the heavy lifting, and for custom parsing or mapping, libraries like MapStruct or GSON can get the job done with much less effort.

### 5. Void method everywhere

![Desktop View](/assets/refactor-blog/void-function.png)

If everything isn’t a String, then it’s a void. I’m not saying void methods are inherently bad, but they don’t give you any feedback on whether a method was successful or not. Without a return object, you’re left guessing what happened.

Rather than relying on try-catch blocks everywhere, returning proper objects makes it easier to track success or failure. It also improves readability by letting you know the purpose of the method just from the return type.

### 6. One class holds more than one responsibilty

![Desktop View](/assets/refactor-blog/class-responsibility.png)

Most of us are familiar with SOLID principles, yet we often forget to implement the “S”—Single Responsibility Principle. Take a look at this Report class: it’s handling both report generation and commission calculations. These responsibilities should be split between a GenerateReportService and a CommissionService.

Bundling multiple responsibilities into one class makes it harder to debug and maintain. When you’re fixing an issue with a PDF report, you don’t want your mind wandering to commission logic.

### 7. Code redundancies everywhere

![Desktop View](/assets/refactor-blog/code-redundant.png)

With methods stretching over 1,000 lines, it’s no surprise that code redundancy was rampant. The same logic was repeated across multiple methods, reducing maintainability and increasing the chance for bugs.

For instance, HttpURLConnection was instantiated in every method in one class, even though it could have been reused through a single global method. Simple refactoring like this can save you from unnecessary redundancy.

### 8. Complex business logic, nested if?

![Desktop View](/assets/refactor-blog/nested-if-else.jpg)

Take a look at this nested if statement. What’s going on here? More than three levels of nesting make it nearly impossible to understand the logic.

There are much cleaner ways to handle business logic, like the Strategy Pattern, using ternary operators, merging similar conditions, or even leveraging Java Streams or Optional objects to deal with null values. Deeply nested if statements only serve to make your code harder to read and maintain.

## Conclusion
Refactoring this Spring Boot project was both challenging and rewarding. From breaking down massive methods to eliminating redundancy, the lessons learned are invaluable. There is no end on optimizing your code, but at least make sure it the code is readable for other people that are going to maintain it. You are not going to hold that project until the it dead ain't it. Everyday is a learning day, while Spring Boot reduces time in development and increases productivity, very bad implementation will give you nightmare. What do you think of this, just lemme know what are the other things to improve in this codebase. I hope this outline provides a glimpse into the challenges and triumphs of cleaning up a messy project. Finally just finished my first blog post, more to come hopefully!!

