# C360 LOS Introduction

EDR's Loan Origination System APIs are a set of application
programming interfaces designed to provide access to EDR services.

The APIs enable you to use your own custom software to create and
manage service requests within Collateral 360, so you can seamlessly
integrate Collateral 360's industry-leading features into your own
familiar due diligence and workflow control software. Using EDR's
APIs can help you achieve time and cost savings, increase efficiency,
and reduce troublesome manual data entry errors between different
systems.

# Business Workflow Overview

The purpose of the LOS APIs is to facilitate the management of
service requests via custom client applications. As a user of
Collateral 360, you or your organization may wish to use your
own software to accomplish or automate common tasks normally
performed through the Collateral 360 Web application's user
interface. The LOS APIs are a starting point for achieving
this goal.

Lending institutions that use Collateral 360 to manage their
real property due diligence processes typically begin by
creating a draft service request via Collateral 360's
service request form (SRF). While in the "draft" status, a
service request is allowed to be incomplete, since a draft
represents work-in-progress.

Depending on your institution's settings in Collateral 360,
a draft service request may only be visible to its author
until the service request is marked as no longer being in
draft status.

A service request typically contains some or all of the
following data:

* Basic loan information (principal, purpose, _etc._).
  
* Details about the borrowers.
  
* A list of real property locations offered as collateral.
  
* Tasks that must be accomplished to complete the underwriting
  and funding decision-making processes. (These are the
  services being requested.)
  
* Records of the individuals responsible for completing the
  aforementioned tasks.

Lending institutions may optionally organize their service
requests into separate _cabinets_, which are simply named
collections of service requests meant for logically grouping
service requests in some meaningful way.

For each collateral location, a set of _services_ can be
requested. (The aggregated collection of services requested
for all collateral properties is from where the name _service
request_ is derived.) Each service represents a task that
must be performed on the property in order to proceed towards
completion of all necessary due diligence. Lending institutions
can customize the list of services, so these will vary between
different users of Collateral 360. However, accomplishing a
services usually results in some form of documentation, such as
a report, affidavit, contract, or other paperwork. These
documents are typically uploaded into Collateral 360 as digital
files, and can then be downloaded and reviewed by any individual
who has been granted permission to do so.

Each service always belongs to a broad category. For example,
services related to environmental due diligence may reside in
the "Environmental" category, while those related to appraising
a property's condition may be in the "Appraisal" category.

> The preceding workflow describes the components of Collateral
> 360 that are covered by the LOS API, and should provide a
> general understanding of the terminology and purpose of the
> API requests described in this document. A broader description
> of the other features and processes supported by Collateral 360
> is not within the scope of this document.



