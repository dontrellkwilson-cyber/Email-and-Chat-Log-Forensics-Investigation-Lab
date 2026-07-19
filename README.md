# Email and Chat Log Forensics Investigation Lab

<p align="center">
  <strong>Analyzing Outlook, Thunderbird, Slack, and Discord Evidence with Paraben’s E3</strong>
</p>

---

## Overview

This lab documents a forensic investigation of email and chat artifacts recovered from a Windows drive image and from imported cloud application data. Using Paraben’s E3, I examined Outlook and Thunderbird email databases, analyzed RFC email headers, sorted attachments, searched for targeted terms and emojis, used OCR to index text in images, and reviewed Slack and Discord communications.

The investigation followed a fictional case involving Beverly Gates, an employee suspected of using company systems and secondary online identities to communicate about illegal drug sales. The purpose of the lab was to identify, correlate, and document evidence from multiple communication platforms.

> **Training Scenario:** All names, accounts, messages, and evidence described in this repository are part of an educational lab environment.

---

## Lab Objectives

- Import a segmented forensic drive image into Paraben’s E3.
- Navigate Outlook and Thunderbird email database structures.
- Review message bodies, metadata, attachments, and RFC headers.
- Identify an email sender’s originating IP address.
- Sort email attachments with E3 Content Analysis.
- Use Advanced Search to filter email and chat records.
- Search for coded terms and emoji-based communications.
- Use OCR to index text contained inside image attachments.
- Examine Slack workspace members, channels, and conversations.
- Examine Discord friends and direct messages.
- Correlate evidence across Outlook, Thunderbird, Slack, and Discord.
- Document findings with focused screenshots.

---

## Skills Demonstrated

- Digital evidence examination
- Email header analysis
- Outlook OST database navigation
- Thunderbird profile analysis
- RFC-formatted email header review
- Attachment triage
- Metadata and timestamp analysis
- Advanced search filtering
- Emoji-based evidence searches
- OCR keyword indexing
- Slack database analysis
- Discord database analysis
- Cross-platform evidence correlation
- Investigative reporting

---

## Lab Environment

| Component | Details |
|---|---|
| Workstation | Windows Server 2019 virtual workstation |
| Forensic Tool | Paraben’s E3 |
| Primary Evidence | `BG_evidence.001` segmented drive image |
| Outlook Database | `bev.gates@outlook.com.ost` |
| Thunderbird Profile | `dfb7v5vd.default-release` |
| Slack Evidence | E3 cloud-data case import |
| Discord Evidence | E3 cloud-data case import |
| Primary Subject | Beverly Gates |
| Investigation Type | Email and chat-log forensics |

---

## Evidence Sources

| Source | Artifact Type | Investigative Use |
|---|---|---|
| Outlook | OST email database | Email content, headers, attachments, timestamps, and targeted searches |
| Thunderbird | Email profile folders | Secondary identity, email headers, emoji searches, and image evidence |
| Slack | Cloud chat database | Workspace members, channels, and coded conversations |
| Discord | Cloud chat database | Friend list, direct messages, emoji searches, and additional conversations |
| Drive image | NTFS filesystem | Source of local communication databases and related artifacts |

---

## Section 1: Hands-On Investigation

### Part 1: Analyze Outlook Email Headers

#### Create the E3 Case and Import the Drive Image

I created an E3 case for the investigation and imported the first segment of the forensic image:

```text
C:\Beverly Gates Evidence\BG_evidence.001
```

E3 detected the remaining image segments and added the NTFS evidence to the case.

I navigated to:

```text
BG_evidence
└── NTFS
    └── Data Triage
        └── E-mail Databases
```

I opened the Outlook database:

```text
bev.gates@outlook.com.ost
```

The relevant Outlook folder structure was located under:

```text
Outlook Offline Storage
└── Root - Mailbox
    └── IPM_SUBTREE
        ├── Inbox
        ├── Sent Items
        ├── Deleted Items
        └── Other Outlook folders
```

---

#### Finding 1: Happy Reminder Email and Timestamps

I opened the email from Karen Jef with the subject:

```text
Re: Happy Reminder
```

The message appeared to discuss an office party theme and did not contain direct evidence of illegal activity. However, it demonstrated the process for reviewing email content and message metadata.

The Properties pane showed:

| Timestamp Type | Value |
|---|---|
| Creation Date | `4/28/2021 7:32:41 AM` |
| Received Date | `4/28/2021 7:32:41 AM` |
| Sent Date | `4/28/2021 7:32:39 AM` |

<p align="center">
  <img src="https://github.com/user-attachments/assets/df60daf2-cb4a-4b86-ad0b-098c71e76363"
       alt="Paraben E3 displaying the Re Happy Reminder Outlook email and its timestamps"
       width="800">
</p>

<p align="center">
  <em>Figure 1: The `Re: Happy Reminder` email displayed with its creation, received, and sent timestamps.</em>
</p>

---

#### Finding 2: Routing IP Address from an RFC Header

I opened the email from Vicky Reed with the subject:

```text
Re: Intricate Solution Job Offer
```

I selected the **RFC Header** viewer and reviewed the message-routing and authentication fields. The routing header contained the following IP address:

```text
209.85.167.54
```

Each mail server can add a `Received` field as a message travels to its destination. An IP address in that chain may identify mail infrastructure used to relay the message, but it should not automatically be treated as the sender's device address or physical location. The full header should be reviewed in order and correlated with authentication results before drawing a conclusion.

<p align="center">
  <img src="https://github.com/user-attachments/assets/f182223f-093d-4198-85e2-7629be29d52d"
       alt="Paraben E3 displaying the RFC header and routing IP information for the job-offer email"
       width="800">
</p>

<p align="center">
  <em>Figure 2: A routing IP address identified in the RFC header of the `Re: Intricate Solution Job Offer` email.</em>
</p>

---

### Part 2: Search for Evidence in the Outlook Database

#### Sort Email Attachments

I ran E3 Content Analysis against the `IPM_SUBTREE` folder:

```text
Content Analysis
└── Sort Data
```

E3 organized the email attachments into file-type categories. I selected the **Graphics** category to review image attachments and their source emails.

Reviewing attachments first can help identify high-value evidence without manually opening every email.

<p align="center">
  <img src="https://github.com/user-attachments/assets/47758961-f2a7-4ef5-8ac7-7b3af0a418c6"
       alt="Paraben E3 Content Analysis showing Outlook graphic attachments organized for review"
       width="800">
</p>

<p align="center">
  <em>Figure 3: Email attachments identified and organized in the Graphics category by E3 Content Analysis.</em>
</p>

##### Attachment Review

I selected each graphic file, reviewed the image in **File View**, and examined the associated email for context.

Most of the reviewed messages appeared consistent with routine workplace communication. Messages involving Beverly and Mr. Harris Malone, also referred to as Alan, used unusually deferential language and referenced an urgent deadline at midnight on April 26, 2021. These details were documented for correlation and were not treated as proof of misconduct by themselves.

---

#### Advanced Search for Beverly and Mr. Harris Malone

I searched Beverly’s Sent Items using the following criteria:

| Search Field | Value |
|---|---|
| Sender | `bev.gates@outlook.com` |
| Recipient | `mr.harris@Intricate365.onmicrosoft.com` |
| Start | `4/26/2021 12:00:00 AM` |
| End | `4/27/2021 12:00:00 PM` |

The search returned several relevant messages, including a final email from Beverly that referenced a **Big Boss**. The phrase was not independently conclusive, but it was documented as potentially relevant when considered with the other communications.

<p align="center">
  <img src="https://github.com/user-attachments/assets/ddd9dfa1-3b40-4db2-ae40-3dcf5cf1a47e"
       alt="Paraben E3 displaying Beverly's Outlook email referencing a Big Boss"
       width="800">
</p>

<p align="center">
  <em>Figure 4: An email from Beverly to Mr. Harris Malone referencing a `Big Boss`.</em>
</p>

---

### Part 3: Search for Evidence in a Slack Database

#### Import the Slack Evidence

I added the following E3 data case as new evidence:

```text
C:\Email Forensics\Beverly_Gates_evidence_Cloud import_04-29-2021_21-05-03
```

I navigated to Beverly’s Slack account and expanded the **Intricate Solutions** workspace.

---

#### Review Workspace Members

I selected the **Members** grid to identify the Slack users connected to Beverly’s workspace.

<p align="center">
  <img src="https://github.com/user-attachments/assets/9343b2fb-33d1-4159-a0f5-f08fdead3deb"
       alt="Paraben E3 displaying members of the Intricate Solutions Slack workspace"
       width="800">
</p>

<p align="center">
  <em>Figure 5: Members identified in the IntricateSolutions Slack workspace.</em>
</p>

---

#### Review Workspace Channels

I selected the **Channels** grid to identify the available Slack communication channels.

<p align="center">
  <img src="https://github.com/user-attachments/assets/eedddc5f-64dc-4020-a102-0a4488622ad4"
       alt="Paraben E3 displaying channels in the Intricate Solutions Slack workspace"
       width="800">
</p>

<p align="center">
  <em>Figure 6: Channels identified in the IntricateSolutions Slack workspace.</em>
</p>

---

#### Search for the Code Words `star dust`

I performed an Advanced Search using:

```text
star AND dust
```

The search returned a conversation between Beverly and Eliot, identified as **Just Eliot**, in the Main Channel. Eliot referenced “star dust,” Beverly reacted strongly, and she instructed him to use Discord.

This message was important because it suggested that:

- `star dust` may have had a specific meaning within the conversation.
- Beverly directed Eliot to continue the discussion outside the company Slack workspace.
- Discord could contain additional context relevant to the investigation.

<p align="center">
  <img src="https://github.com/user-attachments/assets/d42b1a6b-d59d-42b4-b82a-cba684e9e912"
       alt="Paraben E3 displaying the Slack conversation referencing star dust and Discord"
       width="800">
</p>

<p align="center">
  <em>Figure 7: The Slack conversation referencing `star dust` and directing Eliot to continue the discussion on Discord.</em>
</p>

---

## Section 2: Applied Learning

### Part 1: Import and Analyze a Thunderbird Database

The Thunderbird profile was stored in:

```text
dfb7v5vd.default-release
```

Because the profile was embedded in the drive image, I exported it to the E3 case directory and reimported it as a Thunderbird email database.

The Inbox data was imported from:

```text
dfb7v5vd.default-release
└── ImapMail
    └── imap.gmail.com
```

The Sent, Deleted, and Outbox data was imported from:

```text
dfb7v5vd.default-release
└── Mail
    └── Local Folders
```

<p align="center">
  <img src="https://github.com/user-attachments/assets/e381af59-7f4d-4bf3-9af8-ff1ba5c7dbe7"
       alt="Paraben E3 displaying the imported Thunderbird Inbox"
       width="800">
</p>

<p align="center">
  <em>Figure 8: The imported Thunderbird Inbox displayed in Paraben E3.</em>
</p>

---

#### Finding 3: `Well, well, well` Email Header Review

I opened the email with the subject:

```text
Well, well, well
```

I reviewed its RFC header and documented the sender and mail-server information.

| Header Field | Documentation Status |
|---|---|
| Sender email address | Visible in Figure 9 but not transcribed into the original Markdown |
| Mail server name | Visible in Figure 9 but not transcribed into the original Markdown |
| Mail server IP address | Visible in Figure 9 but not transcribed into the original Markdown |

I did not invent these values. They should be copied exactly from the RFC-header screenshot before this finding is treated as fully documented.

<p align="center">
  <img src="https://github.com/user-attachments/assets/4a50d772-4310-47b9-b553-ada326cd3f32"
       alt="Paraben E3 displaying the RFC header of the Well well well Thunderbird email"
       width="800">
</p>

<p align="center">
  <em>Figure 9: The RFC header of the `Well, well, well` email showing the sender and mail-server information.</em>
</p>

---

### Part 2: Search for Evidence in Thunderbird

#### Search for a Maple Leaf Emoji

Senior management suspected that Beverly used emojis as code for drugs. I performed an Advanced Search of the Thunderbird Inbox using the maple leaf emoji.

The result identified an email sent by:

```text
Leo Deforest
```

The message used several emojis that the training scenario treated as possible coded references to drugs. This made Leo a person of investigative interest, but the message alone did not establish his intent or role.

<p align="center">
  <img src="https://github.com/user-attachments/assets/becad251-4152-45bf-a366-22c07f01ae46"
       alt="Paraben E3 displaying the Thunderbird email from Leo Deforest containing emojis"
       width="800">
</p>

<p align="center">
  <em>Figure 10: The email from Leo Deforest containing emojis suspected of representing different drugs.</em>
</p>

---

#### OCR Search for the Word `pills`

I used E3 Content Analysis to index text inside graphic files:

```text
Content Analysis
└── Index Keywords in Images (OCR)
```

After OCR indexing, I performed a keyword search for:

```text
pills
```

The search returned one PNG image containing the word `pills`. The body of the source email did not contain the search term, so the evidence would not have been found through a normal text-only search.

The Thunderbird evidence also revealed:

| Artifact | Finding |
|---|---|
| Secondary email account | `redwitch321@gmail.com` |
| Identity used in messages | Natasha “Red” Maximoff |
| Evidence type | Image explicitly referencing pills |
| Investigative significance | Possible alternate identity and an explicit reference to `pills` requiring contextual review |

<p align="center">
  <img src="https://github.com/user-attachments/assets/d2f08710-e595-4d59-a043-7a9b75fd0ce1"
       alt="Paraben E3 displaying OCR-indexed image evidence containing the word pills"
       width="800">
</p>

<p align="center">
  <em>Figure 11: OCR-indexed image evidence referencing `pills` and messages signed as Natasha “Red” Maximoff.</em>
</p>

---

### Part 3: Search for Evidence in a Discord Database

#### Import the Discord Evidence

I added the Discord E3 data case:

```text
Beverly_Gates_evidence_Cloud import_04-29-2021_20-55-03
```

The Discord account was identified as:

```text
bgates.genius.2345632@gmail.com
(Beverly_G_2345632)
```

---

#### Review Beverly’s Discord Friend List

I selected the **Friends** grid to identify the accounts associated with Beverly’s Discord profile.

<p align="center">
  <img src="https://github.com/user-attachments/assets/f4131ae7-0060-4803-87b2-2635db325afe"
       alt="Paraben E3 displaying Beverly Gates's recovered Discord friend list"
       width="800">
</p>

<p align="center">
  <em>Figure 12: Beverly Gates’s Discord friend list recovered from the imported chat database.</em>
</p>

---

#### Search Discord Direct Messages with Emojis

I searched the Direct Messages folder using:

```text
pill emoji OR wine-glass emoji OR cocktail-glass emoji
```

The search returned multiple messages between Beverly and:

```text
Lena Goodwin
```

Lena used the pill emoji in a request. Beverly replied with a reference to **Mrs. M**, which may connect the Discord conversation to the Maximoff identity found in the Thunderbird evidence. Additional account and message correlation would be needed to confirm that connection.

<p align="center">
  <img src="https://github.com/user-attachments/assets/cfdb455c-c621-4f62-8d29-93570043d5b7"
       alt="Paraben E3 displaying a Discord message from Lena Goodwin using the pill emoji"
       width="800">
</p>

<p align="center">
  <em>Figure 13A: Lena Goodwin requested pills using the pill emoji while communicating with Beverly Gates.</em>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/37a73706-a884-43a8-8118-12a5ae6ed666"
       alt="Paraben E3 displaying Beverly's Discord response referencing Mrs M"
       width="800">
</p>

<p align="center">
  <em>Figure 13B: Beverly’s related Discord message referenced `Mrs. M`, supporting a possible connection to the Mrs. Maximoff identity found in the Thunderbird evidence.</em>
</p>

---

## Section 3: Challenge and Analysis

### Part 1: Search for Additional Outlook Evidence

I returned to Beverly’s Outlook database and performed an Advanced Search for the pill emoji.

The search returned an additional email thread related to the suspected drug-sales communications.

<p align="center">
  <img src="https://github.com/user-attachments/assets/7e29890b-3063-4370-9686-c0f748e475c4"
       alt="Paraben E3 displaying an Outlook email thread returned by a pill-emoji search"
       width="800">
</p>

<p align="center">
  <em>Figure 14: An Outlook email thread returned by the Advanced Search for the pill emoji.</em>
</p>

---

### Part 2: Search for Additional Discord Evidence

I manually reviewed other Discord conversations outside the original emoji-search results and identified another conversation related to suspected drug trafficking.

Document the participants and key content from your results:

| Field | Finding |
|---|---|
| Conversation participants | `JulyRiley_35` and `Beverly_G_2345632` |
| Message from `JulyRiley_35` | `will you help me with 💊` |
| Response from `Beverly_G_2345632` | `yep, write to red.witch231@gmail.com; Tell her you are from me and be nice` |
| Investigative significance | A pill-related conversation that directed the participant to another email address |

The address `red.witch231@gmail.com` is not identical to the Thunderbird address `redwitch321@gmail.com`. The similarity is worth documenting, but the two accounts should not be treated as the same identity without additional corroboration.

<p align="center">
  <img src="https://github.com/user-attachments/assets/118fcd25-f1b6-45f3-9629-be7841f14f92"
       alt="Paraben E3 displaying the JulyRiley 35 Discord message asking for help with pills"
       width="49%">
  <img src="https://github.com/user-attachments/assets/28157d5e-2095-43ea-9964-45c6e3ddcdf9"
       alt="Paraben E3 displaying Beverly's Discord response directing the user to another email address"
       width="49%">
</p>

<p align="center">
  <em>Figure 15: Additional evidence identified during the manual review of Beverly’s Discord conversations.</em>
</p>

---

## Evidence Correlation

| Evidence Source | Finding | Correlation |
|---|---|---|
| Outlook | Communication with Mr. Harris Malone and a reference to a `Big Boss` | Potentially relevant language requiring correlation with other evidence |
| Slack | `star dust` conversation and instruction to use Discord | Suggests coded language and movement to another platform |
| Thunderbird | `redwitch321@gmail.com` account and Natasha “Red” Maximoff identity | Supports a possible alternate identity associated with the evidence set |
| Thunderbird OCR | Image containing the word `pills` | Provides an explicit term hidden inside an attachment |
| Thunderbird | Leo Deforest message containing several emojis | Identifies a person and conversation requiring further contextual review |
| Discord | Lena Goodwin message using the pill emoji | Supports a recurring emoji-based communication pattern |
| Discord | Beverly references `Mrs. M` | May relate to the Maximoff identity, but the connection remains an inference |
| Outlook challenge | Pill-emoji email thread | Extends the recurring emoji pattern to the Outlook evidence |
| Discord challenge | `JulyRiley_35` conversation and referral to `red.witch231@gmail.com` | Adds another pill-related message and a separate, similarly named email address |

---

## Key Findings

### Multiple Communication Platforms Were Used

Relevant communications were distributed across Outlook, Thunderbird, Slack, and Discord. Reviewing only one source would have produced an incomplete picture.

### Evidence Suggested the Use of Alternate Identities

The Thunderbird account `redwitch321@gmail.com` contained messages signed as Natasha “Red” Maximoff. The Discord reference to `Mrs. M` supported a possible connection, but it did not independently prove that the accounts or identities belonged to the same person.

### Emojis and Code Words Were Investigatively Significant

The maple leaf, pill, wine-glass, and cocktail-glass emojis were useful search terms in the context of the training scenario. The Slack phrase `star dust` also appeared potentially coded, although meaning and intent required interpretation from the surrounding conversation.

### OCR Revealed Evidence Hidden from Normal Text Searches

The word `pills` appeared inside an image attachment rather than the email body. E3’s OCR indexing made the evidence searchable.

### Cross-Platform Correlation Strengthened the Findings

The strongest evidence came from combining:

- Email content
- RFC headers
- Message timestamps
- Image attachments
- OCR results
- Slack conversations
- Discord direct messages
- Secondary account identities

---

## Forensic Significance

This lab demonstrated that communication evidence may exist in several layers:

1. Message bodies
2. Email headers
3. Attachments
4. Embedded image text
5. Sender and recipient metadata
6. Timestamps
7. Workspace membership
8. Chat channels
9. Direct messages
10. Alternate accounts and aliases

A complete forensic review should therefore combine database navigation, targeted searching, metadata review, attachment analysis, OCR, and cross-platform correlation. Timestamps from different sources should also be normalized to a documented time zone before constructing a unified timeline.

---

## Results Summary

| Task | Result |
|---|---|
| Outlook database imported | Successful |
| Email timestamps reviewed | Successful |
| Sender IP identified | Successful |
| Attachments sorted | Successful |
| Targeted Outlook search completed | Successful |
| Slack members and channels reviewed | Successful |
| Coded Slack conversation identified | Successful |
| Thunderbird database imported | Successful |
| Thunderbird RFC header reviewed | Header visible; three values still require transcription |
| Emoji email search completed | Successful |
| OCR image search completed | Successful |
| Secondary identity identified | Successful |
| Discord evidence imported | Successful |
| Discord friend list reviewed | Successful |
| Emoji direct-message search completed | Successful |
| Additional Outlook and Discord evidence reviewed | Successful |

---

## Recommended Investigative Practices

- Preserve the original evidence and work from verified forensic copies.
- Record image hashes when they are available.
- Document case names, evidence names, and import paths.
- Capture only the strongest evidence for the report.
- Preserve message context instead of documenting isolated words.
- Record sender, recipient, timestamp, subject, and source application.
- Record the displayed time zone and normalize timestamps before cross-platform comparison.
- Examine both message content and RFC headers.
- Review attachments independently from the email body.
- Use OCR when image-based evidence may contain relevant text.
- Search for alternate identities and account aliases.
- Correlate findings across platforms before drawing conclusions.
- Distinguish direct evidence from investigative inference.

---

> **Ethical Use Notice:** This repository documents an authorized educational forensic investigation. Communication databases and user data should only be examined with proper legal authority or explicit permission.
