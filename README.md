# SurveyCoder MCP server

**Ask questions of your SPSS (.sav) survey data from Claude, and code open-ended answers with AI.**

SurveyCoder is a hosted MCP server for market research. You connect it to Claude, upload a `.sav` file and ask for tables and crosstabs in plain language. A tabulation engine computes the percentages, bases and significance tests; the assistant explains them and builds the charts, the dashboard or the deck.

- **Endpoint:** `https://api.surveycoder.io/mcp`
- **Transport:** Streamable HTTP
- **Auth:** OAuth 2.1 (authorization code with PKCE). No API key.
- **Docs:** <https://docs.surveycoder.io/mcp/>
- **Product page:** <https://surveycoder.io/claude-connector/> · [en español](https://surveycoder.io/conector-claude/)

This repository holds the public documentation of the server. The server itself is a hosted service; there is nothing to install or run.

## Why not just attach the .sav to the chat?

- **The model does not compute the numbers.** The assistant asks SurveyCoder for the table and receives percentages, bases and significance tests already computed. Its job is to explain them.
- **Every percentage comes with its base.** Low bases are flagged, so a difference on a handful of cases is not read as a finding.
- **No SPSS needed.** The server reads the `.sav` with its variable and value labels. You need no SPSS license, no installation and no syntax.
- **Cuts by the answer that matters.** Beyond demographics: promoters against everyone else, people who would buy against those who would not, people who chose a brand against those who did not.

## Connect

### Claude

1. Open **Settings → Connectors**.
2. Find **Survey Coder** in the directory and click **Connect**, or choose **Add custom connector** and enter `https://api.surveycoder.io/mcp`.
3. Sign in to SurveyCoder and authorize access.

Directory listing: <https://claude.ai/directory/connectors/surveycoder>

### Other MCP clients

Any client that supports remote MCP servers with OAuth can use the same endpoint:

```json
{
  "mcpServers": {
    "surveycoder": {
      "type": "http",
      "url": "https://api.surveycoder.io/mcp"
    }
  }
}
```

## What you can ask

```text
I want crosstabs from a .sav file.
Show me Q12, Q13 and Q17 by gender, age and city, with bases and significance.
Compare promoters (9 and 10) with everyone else on Q17 and Q22.
Build a group of people who bought the brand but would not recommend it, and compare it with the rest.
With these results, build a dashboard with charts and the three main findings.
```

And for open-ended questions:

```text
Analyze the .sav I uploaded and propose a coding plan.
Generate the codebook for Q5 and show it to me before coding.
How much would it cost to code the whole project?
```

## How it works

1. Ask for crosstabs. The assistant returns an upload link. Files attached to the chat do not reach SurveyCoder; the `.sav` goes through that link.
2. The assistant checks the file and shows what it will import. Nothing is imported until you confirm. If there is nothing to code, say "tables only".
3. Ask your questions. Each request takes up to twelve questions and up to four banners.
4. Ask for cuts by a key answer. Those groups are built for the request and are not saved to the project.

## Tools

| Area | Tools |
| --- | --- |
| Results and tables | `tabulation_base_prepare`, `tabulation_catalog`, `tabulation_query`, `tabulation_design_get`, `tabulation_design_save`, `tabulation_generate`, `tabulation_status`, `advisor_question_results`, `advisor_question_segments` |
| Projects and files | `projects_search`, `advisor_attention`, `advisor_project_status`, `upload_link`, `inbox_list`, `advisor_analyze_sav`, `plan_apply`, `wave_check` |
| Coding | `advisor_review_question_setup`, `question_config_update`, `question_add_from_file`, `codebook_generate`, `codebook_get`, `codebook_reorganize`, `coding_estimate`, `coding_start`, `refine_suggestions`, `refine_resolve` |
| Deliverables and credits | `deliverable_create`, `deliverables_list`, `deliverable_link`, `credits_overview` |

Full descriptions: <https://docs.surveycoder.io/mcp/>

## Safeguards

- Every tool that changes a project or spends credits works in two steps: a summary first, then the action only after you confirm.
- The server acts with your own SurveyCoder user and your organization's permissions.
- You can revoke access at any time in SurveyCoder, under **Settings → Connected apps**.

## Good to know

- The tables path works with `.sav` files.
- Significance letters between columns appear on category rows and on battery summaries.
- Top 2 Box and mean are only computed when the direction of the scale can be identified; otherwise you get the full distribution.

## Pricing

The connector is free. Coding open-ended answers consumes SurveyCoder credits, one per response; new accounts start with 250 free credits. See <https://surveycoder.io/>.

## Privacy and support

- Privacy policy: <https://surveycoder.io/privacy/>
- Security contact: <https://surveycoder.io/.well-known/security.txt>
- Support: <support@surveycoder.io>

SurveyCoder is made by Insight Genius.
