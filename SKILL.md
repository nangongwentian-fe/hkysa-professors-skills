---
name: hkysa-professors
description: Search the HKYSA faculty database, check university appointment evidence, and save members' shortlists. Use for finding or comparing professors by university, field, topic, rank, or h-index.
---

# Find faculty with HKYSA

Use the `hkysa` MCP tools. Access requires the user's own approved HKYSA account. If a tool asks for sign-in, call `login` and show the returned URL and user code. Ask the user to sign in and approve in their browser, then call `login_status` after they confirm. They can also run `npx @hkysa/mcp login` in a terminal. Keep credentials out of chat.

1. Clarify criteria that would change results. Use `list_facets` to discover available university codes, rank keys, and fields. A research topic such as 大模型 belongs in `q`, while `fields` accepts OpenAlex field names.
2. Search with the user's filters and paginate only as needed. For “找港科大做大模型的助理教授，h-index 10 以上”, use `search_professors` with:

   ```json
   {
     "q": "大模型",
     "universities": ["HKUST"],
     "ranks": ["assistant"],
     "minHIndex": 10,
     "sort": "relevance",
     "pageSize": 20
   }
   ```

   The minimum is inclusive. If results are sparse, try a closely related topic such as “large language models” while preserving the stated rank, university and threshold.

3. Use `get_professor` on the strongest matches to retrieve topic and source evidence. Present a table with name, university, department, rank, h-index, relevant topics, and source links. Use `evidence[].sourceUrl` for appointments and `openalexUrl` for publication metrics when available. Label missing values and give the total and how many results are shown.
4. When the user asks to save results, find the named list with `list_shortlists` or create it with `create_shortlist`. Use `add_to_shortlist` once per selected professor, passing the list ID and the professor ID from search. Report which additions succeeded.

## Coverage and rank

HKYSA covers HKU, CUHK, HKUST, PolyU, CityU, HKBU, LingU and EdUHK. Included appointments comprise chair, full, associate and assistant professors, research assistant professors, teaching/practice/clinical faculty, lecturers and senior lecturers, and emeritus professors. Mainland campuses are counted with their parent universities and tagged in department names. HKU and CUHK “(by courtesy)” appointments may reflect a full-time faculty member's affiliation with an additional department.

Adjunct, visiting, honorary, affiliate, guest, courtesy-only and part-time appointments, postdocs, research fellows and assistants, administrators, and people without a verified faculty title are outside scope. When the university gives only “Prof.”, the person can appear as `unknown` (“职级未注明”). The `professor` filter is for confirmed full professors. University evidence takes precedence for titles and departments. Publication metrics describe research output.
