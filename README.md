
# Assignment B1

## Overview

This repository contains the development and testing of a custom R function designed to streamline data summarization tasks. The function, `summarize_data`, groups a data frame by one column and calculates summary statistics for another column. This assignment demonstrates the application of best practices in function development, documentation, example usage, and rigorous testing.

## Repository Contents

- `assignment.Rmd`: An R Markdown file that includes:
  - **Function Definition**: The `summarize_data` function, designed to be flexible and robust against various types of input.
  - **Documentation**: Uses `roxygen2` style comments to provide clear and helpful information on using the function.
  - **Examples**: Demonstrates the function's usage with the `mtcars` dataset, handling different scenarios like missing data.
  - **Tests**: Contains tests written with the `testthat` package to ensure the function performs as expected under various conditions.
- `assignment.md`: The rendered Markdown output of the `assignment.Rmd` file.
- `README.md`: This document, which provides an overview of the assignment and guidance on how to utilize the repository.

## Getting Started

### Prerequisites

Ensure you have R installed on your machine along with the following R packages:
- `dplyr`
- `testthat`
- `knitr`

You can install these packages using R commands if they are not already installed:

```r
install.packages(c("dplyr", "testthat", "knitr"))
```

### Running the Assignment

1. **Clone the Repository**: First, clone this repository to your local machine using the command:
   ```
   git clone https://github.com/stat545ubc-2024/assignment-b1-guorunhe-design.git
   ```
2. **Navigate to the Repository**: Change directory to the folder containing the cloned repository.
3. **Open the R Markdown File**: Open `assignment.Rmd` in RStudio or your preferred R environment.
4. **Knit the Document**: Use RStudio's knit button or run the knit command to generate a markdown output from the `assignment.Rmd` file. This will execute the function, its documentation, the examples, and the tests.
