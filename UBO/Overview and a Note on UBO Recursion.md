# Ultimate Beneficial Ownership Report API Integration Guide

The following is an integration guide for ultimate beneficial ownership reports.

A quick note on turnaround times. Due to the fact that we are reliant on a person to fulfill any identity verification requirements and fill out the attestation form on their own time, it is very likely that these orders will not fulfill for many hours or days. It's strongly recommended that you take advantage of our order completion webhooks to be notified at an endpoint of your choosing when your UBO order completes.

## A note on UBO Recursion

You may be required to perform additional business reports and UBOs on any beneficial owners that a) are businesses and b) own 25% or more of the first entity. This requirement may also be required on beneficial owners fulfilling the same requirements from the second level, third level, up to level N. This tree of businesses could be potentially very large in extreme cases. RegHub provides a consolidated "HubReport" available as both a PDF and in JSON form for you to provide to end users. Please read more about the hub report on the [retrieve results page.](https://github.com/Reg-Hub/API/blob/main/UBO/4.%20Retrieve%20Hub%20Report.md)

## Performing the Ultimate Beneficial Ownership Report

The general steps are as follows:
1. [Create a draft for review](https://github.com/Reg-Hub/API/blob/main/UBO/1.%20Create%20a%20UBO%20order%20Draft)
2. [Correct any validation issues](https://github.com/Reg-Hub/API/blob/main/UBO/2.%20Correct%20Any%20Validation%20Issues.md)
3. [Submit the order and wait for results](https://github.com/Reg-Hub/API/blob/main/UBO/3.%20Submit%20Order%20and%20Wait%20for%20Results.md)
4. [Retreive hub report](https://github.com/Reg-Hub/API/blob/main/UBO/4.%20Retrieve%20Hub%20Report.md)

The following sections will go into more depth on each of these steps.
