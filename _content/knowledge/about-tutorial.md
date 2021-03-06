---
title: Tutorial - Adding proactivity
weight: 10
---

The tutorial simulates a home security scenario to demonstrate how to add proactivity to an assistant. The Knowledge and Reasoning component is in alpha mode. Make sure that the feature is enabled for your tenant before completing this tutorial. Check that you can access the Knoweldge REST API Swagger documentation from the Watson Assistant Solutions console.

## Home Security scenario

An assistant is designed to proactively warn home owners when the front door to their home opens.  The assistant sends an alert only when it suspects unauthorized access, that is, when an owner is away from home.

John has used his assistant frequently. The assistant has learned about John during these conversations and stores information about John and his home in its knowledge store.  While John is away from his home, the front door opens.  The assistant is notified that the door is open and sends a text message to John to warn him of a potential security breach at his home.

### High-level steps

Step 1. Create a world model for John and his home.

Step 2. Create a proactive agent that subscribes to state change events from the world model. Add functions to the agent to fire a rule when a state change event is received. The rule evaluates a condition and if the condition is true, runs an action.

Step 3. Start the agent locally and test the rule.

Step 4. Push the agent to IBM Cloud and test the rule.

> **What to do next?**<br/>
Learn how to [create world model objects]({{site.baseurl}}/knowledge/create-objects).
