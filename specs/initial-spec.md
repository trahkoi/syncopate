# West Coast Swing Peer Practice Organizer

## Overview
A web application designed to streamline the organization of peer practice sessions for West Coast Swing dancers.

## Core Features

* **Role-Based Joining:** Participants join a session explicitly as either a "Leader" or a "Follower" to maintain role balance and manage rotation logistics.
* **Group Management:** Registered users can create, configure, and manage dedicated practice groups.
* **Frictionless Entry (QR Code):** Generates a QR code for immediate access, allowing dancers to scan and join the group link instantly from their phones.
* **Spotlight Pairing Generator:** An automated engine that generates pairings for spotlight dances. It utilizes the Hungarian algorithm (minimum cost bipartite matching) wrapped in a custom outer loop to handle uneven groups. The primary constraint is to ensure everyone dancing gets exactly one partner per spotlight while minimizing repeat partnerships across the group's history (pairing history is tracked at the group level, persisting across all sessions within that group).

## User Flows

### Scenario 1: First Session Onboarding
1. **Creation:** A registered user creates a new practice group.
2. **Invitation:** The system generates a QR code and join link for the session.
3. **Joining:** Participants scan the code or click the link to enter the lobby.
4. **Registration:**
   - **Role (Required):** The participant must explicitly select Leader or Follower.
   - **Name (Optional):** The participant may enter their name. If left blank, the system automatically assigns a generated alias.

### Session Mechanics
* **Spotlight Triggering:** The group admin/coach acts as the conductor. They manually trigger the generation of the next spotlight pairings when the group is ready.

### Scenario 2: Recurring Sessions (Long-Lived Groups)
1. **Persistence:** Groups exist as long-lived entities, allowing for multiple, separate practice sessions over time.
2. **Subsequent Sessions:** The group owner can initialize a new session under the existing group umbrella.
3. **Re-joining:** Returning participants can rejoin subsequent sessions frictionlessly. Identity is tracked via a persistent browser cookie (no explicit account creation required for dancers), allowing them to drop back in without re-registering their name. Role selection may be confirmed per-session.
4. **Identity Recovery (Lost Cookies):** If a dancer clears their cookies, the coach can initiate a recovery flow from the dashboard. The coach generates a personalized, single-use recovery QR code for that specific dancer profile. Scanning it re-links the dancer's device to their historical data, keeping the pairing algorithm intact.
