**Implement Client Script & UI Policy -- Incident Management in ServiceNow**

A ServiceNow ITSM project that implements UI Policies and JavaScript
Client Scripts to improve Incident data integrity, automate urgency
handling, enforce mandatory assignment, validate submissions, and
prevent unauthorized State changes from list views.

Project Overview

This project was developed under the Naan Mudhalvan Skill Development
Program and focuses on client-side controls for the ServiceNow
Incident (incident) table.

The solution combines:

UI Policy for conditional field behavior

onChange Client Script for automatic urgency handling

onSubmit Client Script for save-time validation

onCellEdit Client Script for list-view governance

The implementation uses the ServiceNow GlideForm (g_form) API and
the UI Policy engine.

Objectives

Automatically set Urgency to High when Impact is High.

Make Assignment Group mandatory for High Impact incidents.

Make Urgency read-only when Impact is High.

Prevent saving a High Impact incident when Assigned To is empty.

Prevent direct inline editing of the State field from the
Incident list.

Allow normal State updates through the standard Incident form.

Technology Stack

Technology                         Purpose

ServiceNow PDI                     Development and testing platform
Incident Table                     Target entity
UI Policy                          Conditional field behavior
GlideForm (g_form)               Client-side form manipulation
JavaScript / ECMAScript 6          Client Scripts
ServiceNow Application Navigator   Configuration
UI Policies Studio                 UI Policy configuration
Client Scripts Editor              Script development
Agile Scrum                        Project methodology

Project Architecture

1. High Impact UI Policy

Condition: Impact == 1 - High

Actions: - Assignment Group → Mandatory - Urgency → Read-only

When the condition becomes false, the configured UI Policy reverse logic
restores the fields to their normal state.

2. onChange Client Script

When Impact changes to High:

function onChange(control, oldValue, newValue, isLoading) {
    if (isLoading || newValue == '') {
        return;
    }

    if (newValue == '1') {
        g_form.setValue('urgency', '1');
        g_form.addInfoMessage(
            'Urgency set to High for High impact incident.'
        );
    }
}

3. onSubmit Client Script

Prevents a High Impact incident from being saved without an Assigned To
value:

function onSubmit() {
    var impactValue = g_form.getValue('impact');
    var assignedToValue = g_form.getValue('assigned_to');

    if (impactValue == '1' && assignedToValue == '') {
        g_form.showErrorBox(
            'assigned_to',
            'Assigned To is mandatory for High impact incidents.'
        );

        return false;
    }

    return true;
}

4. onCellEdit Client Script

Blocks direct inline editing of the Incident State field:

function onCellEdit(sysIDs, table, oldValues, newValue, callback) {
    alert(
        'State cannot be updated using list editing. Please open the Incident.'
    );

    callback(false);
}

Functional Flow

User selects Impact = High
            |
            +----------------------+
            |                      |
            v                      v
     UI Policy Engine       onChange Client Script
            |                      |
            v                      v
 Assignment Group           Urgency = High
    Mandatory               Info Message
            |
            v
      Urgency Read-Only
            |
            v
       User clicks Submit
            |
            v
     onSubmit Validation
            |
       +----+----+
       |         |
       v         v
 Assigned To   Assigned To
   Empty       Provided
       |         |
       v         v
   Block Save  Save Record

Testing

The project was tested against the following scenarios:

Test Case   Scenario                             Result

TC-01       High Impact + Assigned To empty      PASS
TC-02       High Impact + Assigned To provided   PASS
TC-03       Change Impact from High to Medium    PASS
TC-04       Attempt State inline editing         PASS
TC-05       Update State through standard form   PASS

Performance Results

The documented test results reported:

onChange execution: 11.4 ms

onSubmit execution: 13.8 ms

onCellEdit execution: 8.2 ms

Performance threshold: < 50 ms

Advantages

Real-time client-side validation

Automatic urgency handling

Improved Incident data consistency

Reduced accidental priority changes

Prevents unassigned High Impact incidents from being saved

Protects State updates from list-view bypass

Combines low-code UI Policies with JavaScript Client Scripts

Limitations

These controls are client-side. UI Policies and Client Scripts do not
provide the same enforcement for records inserted through REST/SOAP APIs
or background scripts. Server-side Data Policies or other server-side
controls may be required for integrations.

Future Scope

AI-powered technician recommendation

Server-side Data Policy parity

Automated escalation using Flow Designer

Microsoft Teams / Slack notifications

Service Portal and Virtual Agent integration

Predictive Intelligence for Incident assignment

Project Team

Member                              Responsibility

Ghajalakshmi                        Team Lead -- Architecture, UI
Policy, Reverse Logic & Final
Review

Gayathri                            UI Policy Action & onChange Client
Script

Mahalakshmi                         onSubmit Client Script

Project Resources

ServiceNow Developer Portal

ServiceNow Documentation

Atlassian Agile / Burndown
Guide

Naan Mudhalvan

Project Information

Program: Naan Mudhalvan Skill Development Program
Domain: ServiceNow Enterprise Cloud / ITSM
Target Table: Incident (incident)
Methodology: Agile Scrum
Academic Year: 2024--2025

Conclusion

This project demonstrates how ServiceNow UI Policies and Client Scripts
can work together to enforce Incident data integrity at the
user-interface level. The implementation automates high-impact Incident
handling, validates required ownership before saving, and prevents
unauthorized list-view State modifications.
