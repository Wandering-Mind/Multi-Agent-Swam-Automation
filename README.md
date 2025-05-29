# Multi-Agent-Swam-Automation
An advanced Artificial Intelligence automation agency analyzer using a multi-agent swarm setup on my Windows OS. 

## Phas 3: Testing
- **Week 6**:
-   - Test Operations workflows:
    -   -Validate Data  entry automation and inventory sync.
    -   - Ensure reports are generated correctly.
- **Week 7**:
- Test Marketing workflows:
-   -check lead addition and social media scheduling
-   - Monitor engagement metrics for social media posts
---
## Phase 4: Deployment 
**Timeline**: 1 Week 

### Tasks:
- **Week 8**:
-   - Deploy all tested workflows into the production environmen .
-   -Monitor the performence of workflows and gather feedbakc from users.
-   - Make necessary adjustments based on feedback.

---

## Conclusion
by following this roadmap, the agency will effectively implement n8n and Zapier 
workflows, enhancing operational efficiency and productivity across various
business functions. Regular reviews and updates will ensure that the workflows
remain effective and aligned with business goals.

# Link to the source video: https://www.instagram.com/reel/DIEoeqwAFDD/?igsh=aXJjdnJucnFkYXFz


# AGENCY_ANALYZER
# Swarms Transcript 
# (in terminal) / activate environment 

# swarms-zapier % pip install beautifulsoup4
api_key =OPENAI_API_KEY,
max_loops=2, 
verbose=TRUE

)

# Agent 1: Researcher
    researcher=Agent(
        agent_name="Researcher",
        system_prompt="Search online  (via search_google) for the latest n8n and Zapier workflows and use cases in business processes. Collect data on the most effective workflows, their benefits, and implementation steps. Return the findings in a structured JSON format with the key 'Research_findings'.",
        search_function=search_google,
        model="gpt-4-turbo"
        api_key=OPENAI_API_KEY,
        max_loops=3,
        verbose=TRUE
    )
# Agent 2: Analyzer
analyzer = Agent(
    agent_name="Analyzer",
    system_prompt="Read_agency_workspace.json 'research_findings'. Identify the top 30 workflows for n8n and Zapier based on the research findings. Write a detailed analysis of each workflow, including potential use cases, benefits, and implementation steps.$",
    model="gpt-4-turbo",
    api_key=OPENAI_API_KEY,
    max_loops=2,
    verbose=TRUE
)
# Agent 3: Documenter 
documenter = Agent(
    agent_name="Documenter",
    system_prompt="Read agency_workspace.json 'top_workflows' Write detailed reports$",
    model="gpt-4-turbo",
    api_key=OPENAI_API_KEY,
    max_loops=2,
    verbose=TRUE
)
# Agent 4: Coder 
coder = Agent(
    agent_name="Coder",
    system_prompt="Read agency_workspace.json 'top_workflows'. Generate one n8n and one Zapier workflow for each of the top workflows identified in the analysis. The workflows should be detailed, including triggers, actions, and any necessary configurations.$",
    model="gpt-4-turbo",
    api_key=OPENAI_API_KEY,
    max_loops=2,
    verbose=TRUE
)
# Agent 5: Planner 
planner = Agent(
    agent_name="Planner",
    system_prompt="Read agency_workspece.json 'top workflows' and setup_docs.md. Create a detailed plan for implementing the n8n and Zapier workflows in a business environment. The plan should include steps for setup, configuration, testing, and deployment.$",
    model="gpt-4-turbo",
    api_key=OPENAI_API_KEY, 
    max_loops=2,
    verbose=TRUE
)
# Swarm execution 
def run_swarm():
    # Step 1: Research
    research.query = "best n8n zapier agent workflows use cases business pro$cesses"
    research_data = search_google(research_query)
    research_result = researcher.run(json.dumps(research_data))
    workspace = {"research_findings": json.loads(research_result)["Research_findings"]}
    with open(WORKSPACE, f) as f:
        json.dump(workspace, f)

    # Step 2: Analyze
    with open(WORKSPACE, "r") as f:
        analysis_result = analyzer.run(f.read())
    workspace.update(json.loads(analysis_result) if "{ " in analysis result else {"analysis": analysis_result})")
    with open(WORKSPACE, "w") as f:
        json.dump(workspace, f)

    # Step 3: Document
    with open(WORKSPACE, "r") as f:
        doc_result = documenter.run(f.read())
    with open("Setup_docs.md", "w") as f:
        f.write(doc_result)
