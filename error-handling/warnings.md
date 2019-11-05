# Warnings

Every API response will contain an array of warnings in
its top-level `meta` element. This array will be empty
if no conditions that merit a warning occurred while
processing the API request.

Each warning will have the same structure as an error, but
the presence of one or more warnings will not cause the
API response to indicate failure.

Depending on the nature of the condition that caused a
warning, the request may be only partially fulfilled,
or may be fulfilled completely but with user input that
the API server considers to be indicative of a possible
mistake.

As a contrived example, if an API client submits data to
create  a service request with a loan amount of
10,000,000,000 USD, the system might generate a warning
indicating that this amount seems unreasonable, but would
fulfill the request regardless (since loans are
occasionally underwritten for such large amounts).
