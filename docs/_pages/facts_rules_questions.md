---
title: Facts, Rules, and Questions
parent: Beginners Guide to Blawx
nav_order: 4
---
# Facts, Rules, and Questions
{: .no_toc}

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

The vast majority of the encoding that you do in Blawx will be done setting out Facts, Rules, and Questions. They are the starting points for the encoding process.

When you encode laws, you will do it by setting out facts and rules. Then, when you want to use the laws you have encoded, you will use questions.

## Facts
A “Fact” in Blawx represents a thing that is known to be true. If you have a fact, you need to put it in a Fact block, which looks like this:

![fact]({{ site.baseurl }}/img/fact.png)

So for example, let’s say we have some code that represents the idea that Socrates is a human being. That would be placed in a Fact block.

![socrates_human]({{ site.baseurl }}/img/socrates_human.png)

As you will see in the documentation around Categories, you may also make statements about Categories of objects, like saying “A dog is an animal.” These are also treated as facts because they are a thing that is known to be true in your code.

A fact block can hold as many unrelated facts as you would like. And you can have as many different fact blocks in your workspace as is useful.

![socrates and dog]({{ site.baseurl }}/img/socrates_human_dog_animal.png)

It can be helpful to create separate Fact blocks for pieces of information that you will be working with at the same time, so that when you are not using them you can Collapse the fact block and leave your workspace a little easier to read. See the documentation on the Blawx User Interface for details on collapsing and expanding blocks.

If you ask Blawx whether or not something is true that you have stated as a Fact, the answer is always yes.

Typically facts are used at two different stages of the encoding process. First, they are used to set out the terms of the conversation (e.g. “Human is a Category”, “Mortal is a Category”). Then fact blocks take a back seat while rules are written using those terms (e.g. “We know a thing is a mortal if we know that thing is a human.”). Then we come back to using fact blocks to describe a specific fact scenario that we want to ask questions about (e.g. “Socrates is a Human”). Then we use queries to ask questions (e.g. “Is Socrates a Mortal?”).

It can be helpful to separate facts that are always true in the ruleset from facts that are only true in the specific scenario being tested, and multiple fact blocks can be used for that purpose.

## Rules
Rules are the most important part of encoding in Blawx. They are where you describe the simple logical building blocks that Blawx can combine in complicated ways to generate new insights. That is why the technology that Blawx implements is often referred to as “rules-based artificial intelligence”.

A rule has two parts. The first part is one or more statements that can be proved true by the rule, called the “conclusions.” The second part is the “conditions” that must be met for the conclusion to be proved true.

![rule]({{ site.baseurl }}/img/rule.png)

A rule block has one slot for the conclusions, one slot for the conditions, and a field for the rule’s name. The name of a rule is used primarily to allow it to be overridden. To learn more about overriding, see the documentation on Defeasibility.

The name of the rule is also the first thing to display when the rule block is collapsed, making it useful for identifying collapsed rule blocks.

![collapsed rule]({{ site.baseurl }}/img/collapsed_rule.png)

A rule with no conclusions should be a Question, and a rule with no conditions should be a Fact, so a rule block needs both statement connections filled to work properly.

### A Note on If/Then
Some people might express rules as `if conditions then conclusions`. Blawx avoids this for two reasons.

First, we prefer to avoid the words “if” and “then” to describe rules, because people who are familiar with imperative programming languages (which is most of the programming languages out there) will have in their minds a different meaning for “if” and “then.”

In imperative programming, `if conditions then conclusions` means “if right now the conditions are true, then next the computer should do conclusions.”

Blawx is a declarative logic programming tool, not an imperative one. Which means that the rule `if conditions then conclusions` means “if conditions are true, then conclusions are also true.”

Blawx also follows a tradition in declarative logic programming of reversing the order of the conclusion and the condition in declarative rules. So in Blawx, a rule is paraphrased as `we know conclusions are true if conditions are true`.

So when you are writing Facts and Rules, try to remember that in both cases you start what what is, or what might be, true.

## Queries
Once you have encoded rules and described relevant facts, you will want to ask a question. That is what queries are for.

There are two kinds of queries… queries that include variables, and queries that do not.

### Yes or No Queries
A query that does not include variables is a “yes or no” question. An example is “Is Socrates Mortal?”

![is socrates mortal]({{ site.baseurl }}/img/is_socrates_mortal.png)

This question will be answered by the Blawx reasoner with a “Yes” or a “No.”

### Search Queries
A query that includes variables is a “search” question. Blawx attempts to find any objects that it can place in all the variables used so that the all of the statements will be true. If it can’t find any, it will respond “No.” If it can find any examples, it will respond “Yes”, and provide the objects that made the statements true.

So the question “is Socrates Mortal” above, which is a “yes or no” question, can be changed into a search by replacing “Socrates” with a variable. We will name the variable “who”, because it reads better, but the name is unimportant, as long as you use the same variable name everywhere that it is important the rule is referring to the same object.

![who is morta]({{ site.baseurl }}/img/who_is_mortal.png)

If you ask this question of the Reasoner, the answer back will be:

```
who = Socrates
Yes
```

Remember that a query requires all of its blocks connected by the And connectors to be true at the same time in order to find any answers. So make sure you are only asking one question at a time.

## Declarations in Facts and Rule Conclusions vs Rule Conditions and Queries
Declaration blocks (object, category, and attribute declarations) have the usual meaning when used in a fact or the conclusions of a Rule block. But they have a different meaning when they are included in the conditions of a Rule block, or in a query.

A declaration causes the object, category, or attribute to exist when used inside a fact block or the conclusions of a rule. Inside a condition block, they test whether or not those declarations have been made.

So this rule says “we know fruit is a Category if plants are living things.” This rule has the effect of creating a category when its conditions are met.

![conclude fruit]({{ site.baseurl }}/img/conclude_fruit.png)

However, in the next rule, the same Category declaration block “Fruit is a Category” does not create the category fruit, but instead asks whether the category has been created.

![query fruit]({{ site.baseurl }}/img/query_fruit.png)

If the category Fruit has not been defined anywhere (in a fact, or the conclusion of a Rule that had true conditions), then the answer to the query “is Fruit a category” will be no.

This can be confusing, because Blawx still creates a “Known Category”, “Known Object” or “Known Attribute” block in the toolbox regardless of where the relevant block is used. So you may have a Category selector block called “Fruit” while *in the code* the category fruit does not exist.

So it is important to remember the effect of declaration blocks in different locations:

> **In Facts and Rule Conclusions, a new object or new category block or new attribute block means “this thing exists”. In Rules Conditions and Queries, a declaration block means “does this thing exist?”**