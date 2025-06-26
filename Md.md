## Markdown

### The Absolute Basics
  * **Headings**
    Like HTML `<h1>` to `<h6>`. The more `#`, the smaller the heading.

    ```markdown
    # H1 - The Grand Title
    ## H2 - Major Section
    ### H3 - Subsection
    #### H4 - Sub-subsection
    ##### H5 - Tiny Heading
    ###### H6 - Micro Heading
    ```

  * **Emphasis**

      * **Bold:** Make it strong.
        ```markdown
        **This text is bold.**
        __This text is also bold.__
        ```
      * *Italic:* For emphasis, or when you're being a bit sarcastic.
        ```markdown
        *This text is italic.*
        _This text is also italic._
        ```
      * ***Bold and Italic:*** When you REALLY mean it.
        ```markdown
        ***This text is bold and italic.***
        ___This text is also bold and italic.___
        ```
      * \~\~Strikethrough:\~\~ For old ideas, or when you made a typo you want to show.
        ```markdown
        ~~This text is struck through.~~
        ```

  * **Lists: Organize!**

      * **Unordered (Bullet) Lists:** For when order doesn't matter.
        ```markdown
        - Item One
        - Item Two
            - Sub-item A
            - Sub-item B
        * Another way to list
        + Yet another way
        ```
      * **Ordered (Numbered) Lists:** For step-by-step guides (like setting up your dev environment).
        ```markdown
        1. First step
        2. Second step
           3. Sub-step 1
           4. Sub-step 2
        5. Third step
        ```
        *Pro-Tip:* You can use `1.` for all items in an ordered list, Markdown renders them correctly!

  * **Links!**

    ```markdown
    [Visit Google](https://www.google.com)
    [Check out my GitHub profile](https://github.com/your-username "My GitHub Profile")
    ```

  * **Images**
    Just like links, but with an exclamation mark `!`.

    ```markdown
    ![Alt text for the image](https://www.example.com/image.jpg "Optional image title")
    ```

  * **Code**
      * **Inline Code:** For small snippets within a sentence.

        ```markdown
        Use `git push origin main` to push your changes.
        ```

      * **Code Blocks (Fenced Code Blocks):** For multi-line code. Specify the language for syntax highlighting!

        ````markdown
        ```python
        def hello_world():
            print("Hello, Markdown!")
        ````

        ```javascript
        const message = "Code looks good!";
        console.log(message);
        ```

-----

### Beyond the Basics

  * **Blockquotes: For Important Messages or Wise Words**

    ```markdown
    > This is a blockquote.
    >
    > > Nested blockquotes are also possible, for when you're quoting a quote!
    ```

  * **Horizontal Rules**

    ```markdown
    ---
    ***
    ___
    ```

    (Any three or more hyphens, asterisks, or underscores on a line.)

  * **Tables**

    ```markdown
    | Header 1 | Header 2 | Header 3 |
    |----------|:--------:|---------:|
    | Cell 1   | Cell 2   | Cell 3   |
    | Cell 4   | Cell 5   | Cell 6   |
    ```

      * `:` on the left for left-aligned.
      * `:` on both sides for center-aligned.
      * `:` on the right for right-aligned.

-----

### GitHub Flavored Markdown (GFM)

GitHub (and many other platforms) extends standard Markdown

  * **Task Lists (Checkboxes\!):** 

    ```markdown
    - [x] Finish the new feature
    - [ ] Write tests
    - [ ] Deploy to production (gulp!)
    ```

  * **Mentions (`@`):**
    Link directly to GitHub users.

    ```markdown
    Hey @your-teammate, could you review this PR?
    ```

  * **Issue/PR References (`#`): Link to Relevant Discussions**
    Automatically links to issues or pull requests within the same repo.

    ```markdown
    Fixes #123 (closes issue 123)
    See also #456 for related discussion.
    ```

  * **Emojis:**
    Many platforms support `:emoji_name:`.

    ```markdown
    🎉 Awesome work! :tada:
    Got a :bug: to fix.
    ```

    (Check an emoji cheat sheet for names like `:+1:`, `:rocket:`, etc.)

  * **Disabling Markdown Processing (Escaping):**
    Sometimes you want to show Markdown syntax without it rendering. Use a backslash `\`

    ```markdown
    To make text bold, use `\*\*bold\*\*`.
    ```

-----

### Quick Reference Table

| Element         | Markdown Syntax                  | Example                       |
| :-------------- | :------------------------------- | :---------------------------- |
| Heading 1       | `# Heading 1`                    | `# My Awesome Project`        |
| Heading 2       | `## Heading 2`                   | `## Installation`             |
| Bold            | `**text**` or `__text__`         | `**Important!**`              |
| Italic          | `*text*` or `_text_`             | `*Note this*`                 |
| Strikethrough   | `~~text~~`                       | `~~Old idea~~`                |
| Unordered List  | `- item` or `* item` or `+ item` | `- First item`                |
| Ordered List    | `1. item`                        | `1. Step one`                 |
| Link            | `[Text](URL "Title")`            | `[Docs](https://example.com)` |
| Image           | `![Alt Text](URL "Title")`       | `![Logo](/img/logo.png)`      |
| Inline Code     | `` `code` ``                     | `var x = 10;`                 |
| Code Block      | ` ```language ` ` code ``` `     | ` ```js alert("Hi"); ``` `    |
| Blockquote      | `> Quote`                        | `> "Never give up"`           |
| Horizontal Rule | `---` or `***` or `___`          | `---`                         |
| Table           | `\|H1\|H2\|\n\|--\|--\|\n\|C1\|C2\|`      | See example above             |
| Task List       | `- [ ] Task` or `- [x] Task`     | `- [x] Done`                  |
| Mention         | `@username`                      | `@octocat`                    |
| Issue Ref       | `#issue_number`                  | `Fixes #42`                   |
| Emoji           | `:emoji_name:`                   | `:rocket:`                    |