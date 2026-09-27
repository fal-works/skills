---
name: fal-itemized-human-review
description: Build a temporary HTML review page on which the user judges a text artifact item by item, with a binary decision and a one-line note for each item, and then apply the returned result. Useful when the user wants to inspect every small unit of information in a document or plan, such as each list item or sentence, and decide on removal, adoption, or a partial change.
---

# Itemized human review

This skill supports a review that the user performs by hand. The agent divides a text artifact into items, presents them on one HTML page, and receives the user's decision on each item. The agent then applies those decisions to the artifact. The page is a temporary file for the user to read.

The user reads each item in isolation and decides on it independently of the other items.

## Items

An item is the smallest unit that the user can decide on without deciding on another item at the same time. A list element is the typical item. A single sentence inside a paragraph is also an item when the user can decide on it alone. Statements that cannot be decided on separately form one item. As separate items, one of them could be removed while another is kept, and the kept statement would no longer make sense.

An item that overlaps with another item, or that depends on another item, has an agent's note that names the other item by its ID. The user then sees the relation before deciding on either one.

Every statement in the artifact belongs to exactly one item, including a statement between two list elements, such as a transitional sentence.

## Content of each item

- An ID that does not change during the review, such as `A12`. When the review covers several files, the ID begins with a letter assigned to each file.
- The item's location in the artifact when the page is written, such as its line number and section heading.
- The original text, verbatim.
- A summary in the user's language that states what the item says, in one sentence.
- The agent's note, only where it helps the user decide, such as a note that names an overlapping item.
- A control for the binary decision.
- A one-line text field for a note from the user.

The purpose of the review determines what the binary decision means, such as keep or remove, and adopt or reject. The page states that meaning at the top. The initial decision of every item is the option that leaves the artifact unchanged. The user writes in the note field anything that the binary decision cannot express, such as a partial change, a rewording, or a question.

## The page

The page is one HTML file with no external resources. It is readable in both light and dark color schemes. The item data is defined in one array in the script, and the page renders the items from that array. An item whose decision differs from the initial decision is visibly marked.

The page stores the decisions and the notes in `localStorage`, so that the user can close the page and resume later. The storage key includes the file name, so that two review pages do not share state.

The page produces the result as plain text in a read-only text area, together with a button that copies it to the clipboard. The first line of the result names the page file, so that the agent can match the result to the page. After that line, the result lists only the items whose decision the user changed or to which the user added a note, one item per line, in this form:

```text
<ID>: <decision> / <note>
```

A line for an item whose decision is unchanged but that has a note shows the unchanged decision.

After writing the page, check that the script parses, for example by extracting it and running `node --check`. Also check that the number of items on the page matches the number of items into which the agent divided the artifact.

## Saving and delivering the page

The page is saved where the user says. Without such an instruction, it is saved in the current working directory.

The message in which the agent gives the user the page states the path, the meaning of the binary decision, and how to return the result, which is to paste the copied text into the chat. It also briefly states how the artifact was divided into items, and the overlaps and dependencies that the agent's notes name.

## Applying the result

The pasted text is the user's instruction. Each line applies to the item with that ID. Locate each item in the artifact by its original text, because the line numbers can have changed since the page was written. When the artifact has changed and an item's original text is no longer there, report the item instead of guessing its new location or wording.

A decision alone is applied as stated. A note is applied as an instruction about that item. Where a note does not determine the intended change, ask the user about that item and apply the other lines.

After the user has accepted the changes to the artifact, offer to delete the page. Delete it only when the user explicitly agrees.
