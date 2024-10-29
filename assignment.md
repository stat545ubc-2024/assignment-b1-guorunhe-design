Assignment B1
================

## Exercise 1: Make a Function

I will create a function called `summarize_data` that takes a data
frame, groups it by a specified column, and then summarizes a numeric
column by calculating the mean and standard deviation. This function
will handle missing values appropriately and ensure flexibility in its
inputs.

In the function `summarize_data`, the parameters are named to clearly
reflect their roles within the function:

- `data`: This parameter is named `data` to indicate that it should be a
  dataframe, clearly specifying that this is the dataset on which the
  summarization operations will be performed.
- `group_var`: The name `group_var` is used to denote the variable
  within `data` that defines the groups for aggregation. This name makes
  it clear that this parameter should be the name of a column used to
  group the data.
- `summary_var`: Similarly, `summary_var` explicitly indicates that this
  parameter should be the column name of the data that will be
  summarized, making it intuitive for users to understand that this is
  the variable on which summary statistics like mean and standard
  deviation will be calculated.
- `na.rm`: The `na.rm` parameter stands for “NA remove” and is a common
  naming convention in R, which clearly communicates its purpose to
  control the handling of NA (missing) values in the data summarization
  process. Setting it to `TRUE` by default indicates that NA values will
  be removed before computing the summaries, which is a typical
  requirement for statistical calculations to avoid biased or erroneous
  results.

``` r
#' Summarize Data by Group
#'
#' This function takes a data frame, groups it by a specified column,
#' and summarizes a numeric column by calculating the mean and standard deviation.
#'
#' @param data A data frame to be summarized.
#' @param group_var A string specifying the column name to group by.
#' @param summary_var A string specifying the numeric column to summarize.
#' @param na.rm A logical value indicating whether to remove NA values. Defaults to TRUE.
#'
#' @return A data frame with the grouping variable and the calculated mean and standard deviation.
#'
#' @examples
#' summarize_data(mtcars, "cyl", "mpg")
summarize_data <- function(data, group_var, summary_var, na.rm = TRUE) {
  # Check if group_var and summary_var are in the data frame
  if (!all(c(group_var, summary_var) %in% names(data))) {
    stop("Specified group_var or summary_var does not exist in the data frame.")
  }
  
  # Ensure that summary_var is numeric
  if (!is.numeric(data[[summary_var]])) {
    stop("summary_var must be a numeric column.")
  }
  
  data %>%
    group_by(.data[[group_var]]) %>%
    summarise(
      mean = mean(.data[[summary_var]], na.rm = na.rm),
      sd = sd(.data[[summary_var]], na.rm = na.rm)
    ) %>%
    ungroup()
}
```

## Exercise 2: Document your Function

The documentation for `summarize_data` is included within the same code
chunk using `roxygen2` comments.

## Exercise 3: Include Examples

I will demonstrate how to use the `summarize_data` function with a few
examples.

### Example 1: Summarizing Miles Per Gallon by Cylinder Count in `mtcars`

``` r
# Example 1: Summarize mpg by cyl in mtcars
summary_mtcars <- summarize_data(mtcars, "cyl", "mpg")
print(summary_mtcars)
```

    ## # A tibble: 3 × 3
    ##     cyl  mean    sd
    ##   <dbl> <dbl> <dbl>
    ## 1     4  26.7  4.51
    ## 2     6  19.7  1.45
    ## 3     8  15.1  2.56

### Example 2: Handling NA Values

I will introduce some `NA` values into the `mtcars` dataset to see how
the function handles them.

``` r
# Introduce NA values
mtcars_na <- mtcars
mtcars_na$mpg[c(1, 5, 10)] <- NA

# Summarize with na.rm = TRUE
summary_na_rm <- summarize_data(mtcars_na, "cyl", "mpg", na.rm = TRUE)
print(summary_na_rm)
```

    ## # A tibble: 3 × 3
    ##     cyl  mean    sd
    ##   <dbl> <dbl> <dbl>
    ## 1     4  26.7  4.51
    ## 2     6  19.6  1.64
    ## 3     8  14.8  2.44

``` r
# Summarize with na.rm = FALSE
summary_na_not_rm <- summarize_data(mtcars_na, "cyl", "mpg", na.rm = FALSE)
print(summary_na_not_rm)
```

    ## # A tibble: 3 × 3
    ##     cyl  mean    sd
    ##   <dbl> <dbl> <dbl>
    ## 1     4  26.7  4.51
    ## 2     6  NA   NA   
    ## 3     8  NA   NA

## Exercise 4: Test the Function

Formal tests are essential to ensure that the function behaves as
expected under various conditions. I will use the `testthat` package to
write these tests.

``` r
test_that("summarize_data works correctly with no NAs", {
  result <- summarize_data(mtcars, "cyl", "mpg")  # Perform the operation
  expect_true(is.data.frame(result))  # Validate the result is a dataframe
  expect_equal(nrow(result), length(unique(mtcars$cyl)))  # Compare row count to unique groups
  expect_true(all(c("mean", "sd") %in% names(result)))  # Ensure 'mean' and 'sd' are in column names
})
```

    ## Test passed 🥇

``` r
test_that("summarize_data handles NA values correctly when na.rm = TRUE", {
  mtcars_na <- mtcars
  mtcars_na$mpg[1:3] <- NA  # Introduce NA values into the dataset
  result <- summarize_data(mtcars_na, "cyl", "mpg", na.rm = TRUE)  # Summarize data with NA removal
  expect_true(all(!is.na(result$mean)))  # Check for absence of NAs in 'mean'
  expect_true(all(!is.na(result$sd)))  # Check for absence of NAs in 'sd'
})
```

    ## Test passed 😀

``` r
test_that("summarize_data handles NA values correctly when na.rm = FALSE", {
  mtcars_na <- mtcars
  mtcars_na$mpg[1:3] <- NA  # Introduce NA values into the dataset
  result <- summarize_data(mtcars_na, "cyl", "mpg", na.rm = FALSE)  # Summarize data without removing NAs
  expect_true(any(is.na(result$mean)))  # Expect NAs to be present in 'mean'
  expect_true(any(is.na(result$sd)))  # Expect NAs to be present in 'sd'
})
```

    ## Test passed 🎉

``` r
test_that("summarize_data throws an error for non-numeric summary_var", {
  mtcars_test <- mtcars %>%
    mutate(gear_factor = as.factor(gear))  # Convert 'gear' to a factor to create a non-numeric summary variable
  expect_error(summarize_data(mtcars_test, "cyl", "gear_factor"))  # Expect error due to non-numeric data
})
```

    ## Test passed 🥳

``` r
test_that("summarize_data throws an error when group_var does not exist", {
  expect_error(summarize_data(mtcars, "nonexistent_var", "mpg"))  # Expect error due to non-existent group variable
})
```

    ## Test passed 🌈
