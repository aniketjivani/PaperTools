Inspired by  [Paul Constantine's Paper Workflow](https://github.com/paulcon/pauls-workflow)
Some changes to include SWQU specific files (csem.bib etc) and Overleaf authoring quirks
Other folders will also change as we use this more and more.

Required:
1. Some distribution like MacTeX
2. VSCode Extension called LaTeX workshop (this is more for convenience)


Rough Workflow:

1. Write paper with include links

2. Keep synced with GitHub and git locally. So write on Overleaf but pull from it on local machine to process all files appropriately as necessary. Sync changes with GitHub to keep track of what was changed

3. Have multiple bib files (autogen from Mendeley + Local)

4. Submit paper

5. Perform revisions. Before this merge all include files into one big LaTeX files (because `latexdiff` won't work with `include` commands!:( ) Apparently using `latexpand main.tex > new_article.tex` ?

6. Use `bibexport` to merge multiple bib files into one. Example usage:a) Produce aux file with LaTeX workshop and then:
```bash
bibexport -o extracted.bib myarticle.aux
```

7. Now use `latexdiff` to produce diffed manuscript. Use this for revised submissions.

8. Strip comments out of the main tex file and submit it along with revised submission PDF (AGU asks for this when submitting revision so they can use it if accepted). Use `latexpand` to strip comments too eh? Thanks StackExchange :) https://tex.stackexchange.com/a/271460 (reproducing answer below):

`latexpand` is a tool to expand include/input directives which also removes comments. It can also be tweaked to keep end-of-line comments because sometimes they are meaningful. In a second step remove lines with only `%`:

```bash
$ latexpand --empty-comments mytexfile.tex > mytexfile-stripped.tex
```

Remove lines with only `%` and whitespace:
```bash
$ sed -i '/^\s*%/d' mytexfile-stripped.tex
```
and squeeze blank lines (`cat -s` might have problems with window line endings and on os solaris `-s` has a different meaning)

```bash
$ cat -s mytexfile-stripped.tex | sponge mytexfile-stripped.tex
```


9. Hopefully ready to submit?


