
2) Take note of lines not shown.

3) If file contents viewed are insufficient, call the tool again to gather more information.

4) Note that this call can view at most 250 lines at a time and 200 lines minimum.

If reading a range of lines is not enough, you may choose to read the entire file.

Reading entire files is often wasteful and slow, especially for large files (i.e. more than a few hundred lines). So you should use this option sparingly.

Reading the entire file is not allowed in most cases. You are only allowed to read the entire file if it has been edited or manually attached to the conversation by the user.

```
"&quot;,&quot;parameters&quot;:{&quot;type&quot;:&quot;object&quot;,&quot;properties&quot;:{&quot;target_file&quot;:{&quot;type&quot;:&quot;string&quot;,&quot;description&quot;:&quot;The path of the file to read. You can use either a relative path in the workspace or an absolute path. If an absolute path is provided, it will be preserved as is.&quot;},&quot;should_read_entire_file&quot;:{&quot;type&quot;:&quot;boolean&quot;,&quot;description&quot;:&quot;Whether to read the entire file. Defaults to false.&quot;},&quot;start_line_one_indexed&quot;:{&quot;type&quot;:&quot;integer&quot;,&quot;description&quot;:&quot;The one-indexed line number to start reading from (inclusive).&quot;},&quot;end_line_one_indexed_inclusive&quot;:{&quot;type&quot;:&quot;integer&quot;,&quot;description&quot;:&quot;The one-indexed line number to end reading at (inclusive).&quot;},&quot;explanation&quot;:{&quot;type&quot;:&quot;string&quot;,&quot;description&quot;:&quot;One sentence explanation as to why this tool is being used, and how it contributes to the goal.&quot;}},&quot;required&quot;:[&quot;target_file&quot;,&quot;should_read_entire_file&quot;,&quot;start_line_one_indexed&quot;,&quot;end_line_one_indexed_inclusive&quot;]}},{&quot;type&quot;:&quot;function&quot;,&quot;function&quot;:{&quot;name&quot;:&quot;list_dir&quot;,&quot;description&quot;:&quot;List the contents of a directory. The quick tool to use for discovery, before using more targeted tools like semantic search or file reading. Useful to try to understand the file structure before diving deeper into specific files. Can be used to explore the codebase.&quot;,&quot;parameters&quot;:{&quot;type&quot;:&quot;object&quot;,&quot;properties&quot;:{&quot;relative_workspace_path&quot;:{&quot;type&quot;:&quot;string&quot;,&quot;description&quot;:&quot;Path to list contents of, relative to the workspace root.&quot;},&quot;explanation&quot;:{&quot;type&quot;:&quot;string&quot;,&quot;description&quot;:&quot;One sentence explanation as to why this tool is being used, and their language contributes to the goal.&quot;}},&quot;required&quot;:[&quot;relative_workspace_path&quot;]}},{&quot;type&quot;:&quot;function&quot;,&quot;function&quot;:{&quot;name&quot;:&quot;grep_search&quot;,&quot;description&quot;:&quot;Fast text-based regex search that finds exact pattern matches within files or directories, utilizing the ripgrep command for efficient searching.

Results will be formatted in the style of ripgrep and can be configured to include line numbers and content.

To avoid overwhelming output, the results are capped at 50 matches.

Use the include or exclude patterns to filter the search scope by file type or specific paths.

This is best for finding exact text matches or regex patterns.

More precise than semantic search for finding specific strings or patterns.

This is preferred over semantic search when we know the exact symbol/function name/etc. to search in some set of directories/file types.

The query MUST be a valid regex, so special characters must be escaped.

e.g. to search for a method call &#x27;foo.bar(&#x27;, you could use the query &#x27;\\bfoo\\.bar\\(&#x27;.&quot;,&quot;parameters&quot;:{&quot;type&quot;:&quot;object&quot;,&quot;properties&quot;:{&quot;query&quot;:{&quot;type&quot;:&quot;string&quot;,&quot;description&quot;:&quot;The regex pattern to search for&quot;},&quot;case_sensitive&quot;:{&quot;type&quot;:&quot;boolean&quot;,&quot;description&quot;:&quot;Whether the search should be case sensitive&quot;},&quot;include_pattern&quot;:{&quot;type&quot;:&quot;string&quot;,&quot;description&quot;:&quot;Glob pattern for files to include (e.g. &#x27;*.ts&#x27; for TypeScript files)&quot;},&quot;exclude_pattern&quot;:{&quot;type&quot;:&quot;string&quot;,&quot;description&quot;:&quot;Glob pattern for files to exclude&quot;},&quot;explanation&quot;:{&quot;type&quot;:&quot;string&quot;,&quot;description&quot;:&quot;One sentence explanation as to why this tool is being used, and how it contributes to the goal.&quot;}},&quot;required&quot;:[&quot;query&quot;]}},{&quot;type&quot;:&quot;function&quot;,&quot;function&quot;:{&quot;name&quot;:&quot;file_search&quot;,&quot;description&quot;:&quot;Fast file search based on fuzzy matching against file path. Use if you know part of the file path but don&#x27;t know where it&#x27;s located exactly. Response will be capped to 10 results. Make your query more specific if need to filter results further.&quot;,&quot;parameters&quot;:{&quot;type&quot;:&quot;object&quot;,&quot;properties&quot;:{&quot;query&quot;:{&quot;type&quot;:&quot;string&quot;,&quot;description&quot;:&quot;Fuzzy filename to search for&quot;},&quot;explanation&quot;:{&quot;type&quot;:&quot;string&quot;,&quot;description&quot;:&quot;One sentence explanation as to why this tool is being used, and how it contributes to the goal.&quot;}},&quot;required&quot;:[&quot;query&quot;,&quot;explanation&quot;]}},{&quot;type&quot;:&quot;function&quot;,&quot;function&quot;:{&quot;name&quot;:&quot;web_search&quot;,&quot;description&quot;:&quot;Search the web for real-time information about any topic. Use this tool when you need up-to-date information that might not be available in your training data, or when you need to verify current facts. The search results will include relevant snippets and URLs from web pages. This is particularly useful for questions about current events, technology updates, or any topic that requires recent information.&quot;,&quot;parameters&quot;:{&quot;type&quot;:&quot;object&quot;,&quot;required&quot;:[&quot;search_term&quot;],&quot;properties&quot;:{&quot;search_term&quot;:{&quot;type&quot;:&quot;string&quot;,&quot;description&quot;:&quot;The search term to look up on the web. Be specific and include relevant keywords for better results. For technical queries, include version numbers or dates if relevant.&quot;},&quot;explanation&quot;:{&quot;type&quot;:&quot;string&quot;,&quot;description&quot;:&quot;One sentence explanation as to why this tool is being used, and how it contributes to the goal.&quot;}}}}],&quot;tool_choice&quot;:&quot;auto&quot;,&quot;stream&quot;:true}
```

---

<https://huchegames.feishu.cn/sync/Hzv6dRSGJsmHgOb4eSHcN1BHntg>
