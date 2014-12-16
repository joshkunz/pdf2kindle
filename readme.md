## pdf2kindle

`pdf2kindle` is a command-line tool that automates the process of mailing
a pdf to your [special kindle email address][special]. This tool is
specifically designed to be used with scientific papers, which usually
require some pre-processing before being viewed on a kindle. We use
the excellent [`k2pdfopt`][k2] to perform this pre-processing.

### Running

Make sure you have a copy of `k2pdfopt` [(here)][k2_down] installed in
somewhere in your path, along with the `pdf2kindle` file. Before running
the tool you need to set 4 environment variables that tell `pdf2kindle` how
to mail your pdf, and where to mail it to:

| | |
|---|---|
| `PDF2KINDLE_FROM` | The 'From:' address of the mail, i.e. your email address. For example: `someperson@gmail.com` |
| `PDF2KINDLE_SMTP_HOST` | The address of the host that delivers your mail. For example: `smtp.gmail.com` |
| `PDF2KINDLE_SMTP_PORT` | The port the above host uses to receive mail. For Example: `465` |
| `PDF2KINDLE_KINDLE` | Your secret kindle address. If you don't know what this is, you can [learn about it here][special]. For example: `someperson@kindle.com` |

Now, you invoke the tool:

```bash
$ env PDF2KINDLE_FROM="someperson@gmail.com" \
      PDF2KINDLE_SMTP_HOST="smtp.gmail.com" \
      PDF2KINDLE_SMTP_PORT="465" \
      PDF2KINDLE_KINDLE="someperson@kindle.com" \
  pdf2kindle paper.pdf
```

If you want to invoke the tool more easily, you can add something like the following
to your `~/.bashrc` file:

```bash
export PDF2KINDLE_FROM="someperson@gmail.com"
export PDF2KINDLE_SMTP_HOST="smtp.gmail.com"
export PDF2KINDLE_SMTP_PORT="465"
export PDF2KINDLE_KINDLE="someperson@kindle.com"
```

Then you can invoke it like this: 

```bash
$ pdf2kindle ./paper.pdf
```

### Usage Notes

1. The tool currently assumes that your SMTP host requires TLS before authentication.
   This assumption appears to be pretty standard, but it may not be correct in
   all cases. If you run into weird SMTP bugs, feel free to post an issue.
2. The tool currently optimizes the processed pdf output for the kindle 'paperwhite'.
   this will likely be just fine, but if you want to tune it some more you can
   look at the documentation for `k2pdfopt`. I'd be happy to accept patches
   to add an easy model selector to the tool.
3. The tuning I do to the pdfs works well in about 84% of the cases I've tried.
   Sometimes, figures will get messed up, or text sizes will be wrong. `k2pdfopt`
   especially hates centered figures in two-column papers. Feel free to try and
   tune the system better, or submit issues with pdfs that cause degenerate behaviour,
   I'd love to make the pre-processing better.

  [special]: https://www.amazon.com/gp/help/customer/display.html/ref=kinw_myk_pd_ln?ie=UTF8&nodeId=200767340#assignemail
  [k2]: http://www.willus.com/k2pdfopt/
  [k2_down]: http://www.willus.com/k2pdfopt/download/
