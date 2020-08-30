
# test-docopt

<!-- badges: start -->
<!-- badges: end -->

The goal of the repo test-docopt is to show inconsistent behavior of Rscript 
argument parsing on Windows.

## How to reproduce

**locally**

Run the command in `.github/workflows/main.yaml` with escaped double quotes:

```r
Rscript script --style_pkg=styler "--style_transformers=tidyverse_style(scope = \"none\")" file1 file2
```

and observe the output. The script only prints the parsed command line options
`print(commandArgs(TRUE))`, which are parsed in an unexpected way on Windows,
e.g. for the above command:
```
Rscript analysis/script --style_pkg=styler "--style_transformers=tidyverse_style(scope = \"none\")" file1 file2
[1] "--style_pkg=styler"                                                 
[2] "--style_transformers=tidyverse_style(scope = \" none\\) file1 file2"```
```

Expected output:

```
Rscript analysis/script --style_pkg=styler "--style_transformers=tidyverse_style(scope = \"none\")" file1 file2
[1] "--style_pkg=styler"                                                 
[2] "--style_transformers=tidyverse_style(scope = \" none\")"
[3] "file1"                                             
[4] "file2"  
```


**GitHub Actions**

Fork the repo and push it to GitHub and see the output of the tasks `reprex-1`,
`reprex-2` and `reprex-3`.