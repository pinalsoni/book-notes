



## Gherking Basics
###Feature description:

- Summary documentation about a group of tests
- Line cannot begin with ‘Scenario’, ‘Background’ and ‘Scenario Outline’
- Description is optional (?)
- Naming feature file: Use feature description with a standardized naming scheme such as all lower case with connecting underscores
- Template for describing a feature: Feature injection template… In order to <goal>, As a <stakeholder>, I want <a feature>

###Scenarios (and Steps)

- Expresses the (WHAT) behavior we want
- 1:N Feature:Scenario
- All scenarios collectively provide expected behavior of the feature itself
- Scenario should be testing meaningful functionality of your system
- Different scenarios help describe how feature as a whole should behave in different circumstances
- Use scenarios to explore edge cases and different paths through a feature
- Scenario pattern: similar to (Arrange, Act, Assert) => context, event and an outcome
- Make scenarios stateless (one scenario should not depend on outcome of others)
- Scenario lines are steps; we can add more steps to each Given, When and Then section of the scenario using the keywords And and But
- When modifying existing scenarios, check the name that still make sense, update description to make sure what is being tested is described correctly. This helps avoid confusion
- Put a #language comment on the first line of a feature file; defaults to English unless explicitly configured to a different language setting; setting is applied to all scenarios in a feature file
- Cucumber doesn’t actually care which of the keywords(given, when, then, and, but) are used; the choice is simply there to help you create the most readable scenario; Only thing that matters is that everybody understands what’s communicated by using these keywords

     ####Naming scenarios:

    - Focus on writing a concise, expressive name - when tests break, it’s the failing scenario’s name that helps you understand breakage
    - Helps avoid reading individual steps to understand what are the scenario is testing
    - Well-composed scenario name will still make sense in case of adding extra steps in future
    - Avoid putting anything about the outcome of the scenarios into the name and concentrate on summarizing the context and event of the scenario
    - When modifying existing scenarios, check the name that still make sense, update description to make sure what is being tested is described correctly. This helps avoid confusion

###Concrete Examples

Help everyone in the team understand what we want the software to do and hence facilitates better communication between developers and stakeholders which further reduces waste.

- Focus building concrete examples
- Use real world examples to describe the desired behavior of the system we want to build
- Stay grounded in language and terminology that makes sense to our stakeholders
- Look for precision in acceptance criteria
- Beware of vague statements in ACs that leave a lot of room for interpretation
- Precise ACs help both devs and stakeholders understand what we are going to build; help remove ambiguity
- Examples stimulate our imagination, enabling us to explore and discover edge cases we might otherwise have found later
- Examples help us turn acceptance criterion into an acceptance test
- Much easier to validate against the running system than vague requirement statements


Automated acceptance tests generally try to simulate user interactions with the system, and the step definitions are where you’ll tell cucumber how you want it to poke around with your system.

##Step Definitions: Focuses on HOW

### Matching Step:

    - Steps are expressed as plain text
    - Cucumber scans the text of each step for patterns that recognizes, which are defined using regexes
    - Annotations: Just there for extra documentation to help express the intent of each step or step def
    - Will match any gherkin step as long as the reg ex matches the main text of the step
    - Choose wordings of steps carefully to avoid ambiguity. Focus on communicating what exactly the steps will do when executed.

### Specificity vs. Flexibility

- Specific scenarios are great but limit reuse. To enable this and making scenarios much more flexible, capture arguments in scenarios.
- In regex, two features that can help us with making scenarios  flexible are: 1. Capture groups 2. Wildcards

### Capture Arguments in Step Definitions:

#### Capture groups
Surround part of a reg ex with parantheses. Used to highlight particular parts of a pattern that you want to lift out of the matching text and use

Wild cards
1. Alternation:
..* Provide different options separated by a pipe
..* Useful when there are fixed set of values that you want to accept in your step def

2. The Dot:
..* Metacharacter, matches any single character
..* Can be escaped by preceding them with a backslash
..* NOTE: $ itself is a meta character and hence it is preceeded with backslashes

3. The Star: (greedy operator)
..* Repeatition - any number of times

4. Character classes
..* Allows you to tell the regex expression engine to match one of a range of characters
 ..* Simply place all of the chars you would accept inside square brackets
..* Continuous range of characters: Use hyphen
..* Shorthand character classes:
        \d  - shorthand for [0 - 9]
        \w - shorthand for word; underscore and digits are included but not hyphens
        \s - whitespace character, typically [\t\r\n]
        \b - word boundary: anything that is not a word character is a word boundary
        \D - any character except a digit
        _The Plus_:‘+’ modifier means at least once

#### Multiple Captures:
..* OK if we use multiple capture group in our regex
..* Cucumber will pass an argument in our method for every capture group in regex
..* Flexibility:
    - Encourage our feature authors to be consistent about the nouns and verbs
    - Express as naturally as possible; usage of slightly different phrasing to mean exactly the same thing
..* The question mark modifier:
- Use when you don’t care about the odd character in your match such as singular or plural
- Question mark modifier means zero or one times
- Makes the preceding character optional

#### Noncapturing Groups:
..* Helps recognize multiple phrases
..* The ?: at the start of the group marks it as noncapturing, meaning Cucumber won’t pass it as an argument to our block.

#### Anchors:
    - Steps start with a ^ and ends with a $
    - Not mandatory
    - If committed, step definitions become much more flexible; perhaps too flexible resulting in overmatching multiple scenarios
    - Generally, it’s best to keep your regular expressions as tight as you can so that there’s less chance of two step definitions clashing with each other.
    - That’s why the snippets that Cucumber generates for undefined steps always include the anchors. Still, leaving off the anchors is a trick worth knowing about that can sometimes come in handy.

#### Reporting Results
Cucumber communicates results by either throwing or not throwing an exception. Statuses could be one of the following:
    - Failed: Assertion failure
    - Pending: Under construction (In progress…)
    - Undefined: Matching step definition has not yet been defined
    - Skipped:
    - Passed

Pending vs. Undefined status:
Pending step definitions are halfway through being implemented; it marks the step as pending (yellow); sceanrio will be stopped and the rest of the steps will be skipped or marked as undefined. Undefined step definitions are the ones that do not exist yet (cucumber failed to find those during execution)

    - Pending step definition is identified by throwing a PendingException
    - Can be used to signal teammates that you’re in the middle of working on something and will flush out the step definition later.
    - Pending scenarios can become our to-do list for the work we do
