Save the current conversation to the /history directory with the following requirements:

1. **Filename Format**: `/history/YYYY-MM-DD-HH-MM-short-task-summary.md`
   - Use bash to get the current timestamp: `date +"%Y-%m-%d-%H-%M"`
   - Create a brief, hyphenated summary of the conversation topic (3-6 words)
   - Example: `2025-11-28-13-53-basic-agentic-loop-implementation.md`
   - **IMPORTANT**: Use the Bash tool to run `date +"%Y-%m-%d-%H-%M"` to get the correct timestamp format

2. **File Structure**: Format as markdown with these sections:
   - Title: `# Conversation: [Descriptive Title]`
   - Metadata: Date and Topic
   - Content organized by conversation sections with:
     - `## [Section Name]` headers for major topics
     - `**User:**` and `**Assistant:**` labels for each exchange
     - Properly formatted code blocks with language tags
     - Blockquotes (`>`) for user messages
     - Preserve all important details, explanations, and code

3. **Content Guidelines**:
   - Capture the full conversation flow
   - Maintain all technical details and code examples
   - Use clear section breaks (`---`) between major topics
   - Include a summary section at the end
   - Preserve the reasoning and explanations provided

4. **Process**:
   - Review the conversation to identify key topics and natural sections
   - Create a descriptive filename that captures the main topic
   - Format the content for readability
   - Save to `/history/` directory
   - Confirm the save with the full path

Reference the example format at: /history/2025-11-28-13-53-basic-agentic-loop-implementation.md
