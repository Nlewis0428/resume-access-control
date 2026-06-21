# Resume Access Control System

An invite-only resume delivery system. Visitors complete a request form capturing name, email, organization, and reason for interest. Submissions are logged and routed to the owner via SMTP. The full resume PDF is never publicly exposed — access is controlled and tracked.

## Problem

A public resume link gets scraped, indexed, and shared out of context. There's no record of who requested it or why, and no way to revoke access once it's out.

## Architecture

- A request form (Name, Email, Organization, Reason for Interest) collects visitor details before any document is served
- Submissions are logged for an auditable record of every request
- Form delivery routes through authenticated SMTP rather than the server's default mail function, for reliable delivery and lower spam-folder risk
- The resume PDF itself lives outside the publicly indexed path — it's never linked directly, only served through the gated request flow
- External access runs through a tunnel with no open inbound ports, consistent with the rest of the infrastructure

## Stack

WordPress · Contact Form 7 · Flamingo · WP Mail SMTP · Cloudflare Tunnel

## Outcome

Controlled resume distribution with full submission logging and zero public PDF exposure.

## Notes

`access-gate-example.php` is an illustrative pattern, not the live implementation — it shows the general approach (validate submission, then serve the file) rather than the actual production logic. `contact-form-fields.md` documents the form structure, not an export of the real form. Fill in real SMTP values only in a local `.env`, never committed.
