# Implementing Pagination

## Introduction

Pagination is an essential feature in web applications, especially when dealing with large datasets. It helps improve performance by loading a manageable number of records at a time, such as the top 10 or 20 entries, rather than overwhelming the user and the system with millions of records at once.

## Objectives
By the end of this lesson, you will be able to:

- Understand the importance of pagination in web applications.
- Implement basic pagination using the Kaminari gem in a Ruby on Rails application.
- Customize pagination with Bootstrap for better UI integration.

## Why Pagination?
Imagine an application that has access to over a million records. Loading all these records on a single page is not practical as it can lead to significant delays in page rendering and can overwhelm the user with too much information at once. Pagination allows us to break this data into smaller, more manageable chunks, enhancing both user experience and application performance.

## Pagination with Kaminari
[Kaminari](https://github.com/kaminari/kaminari) is a popular Ruby gem used for pagination in Rails applications. It is simple to integrate and offers a lot of flexibility for customization. Here's how to set it up:

### Step 1: Add the Kaminari Gem
First, add Kaminari to your Gemfile:

```ruby
gem 'kaminari'
```

Then, run `bundle install` to install the gem.

### Step 2: Configure Kaminari in Your Controller
In your controller, decide which model you want to paginate. For example, if you are paginating articles, you might have something like this:

```ruby
def index
  @articles = Article.page(params[:page]).per(10)
end
```
This code fetches 10 articles per page based on the page parameter from the URL.

### Step 3: Add Pagination Controls to the View
To add pagination controls in the view, use the paginate helper provided by Kaminari:

```erb
<%= paginate @articles %>
```

## Integrating with Bootstrap
To style your pagination controls with Bootstrap, you'll need to customize Kaminari's views. Fortunately, Kaminari provides a generator that makes this easy:

```bash
rails g kaminari:views bootstrap4
```

This command generates views that are compatible with Bootstrap 4.

## Under the Hood: Raw SQL

When you use the Kaminari gem to paginate records, it modifies the underlying SQL query to limit the number of records returned per page and to skip the records of previous pages. Here's how the SQL might look when you paginate articles with `Article.page(params[:page]).per(10)`.

Assuming params[:page] is set to 2 (meaning the user is on the second page), the SQL query generated would typically look something like this:

```sql
SELECT  "articles".* FROM "articles" LIMIT 10 OFFSET 10
```

### Explanation:
- `SELECT "articles".* FROM "articles"`: This part of the query fetches all columns of the articles from the articles table.
- `LIMIT 10`: This clause limits the number of records returned by the query to 10. In the context of pagination, this corresponds to the number of articles you want to display per page.
- `OFFSET 10`: This clause skips the first 10 records. This is calculated as `(page_number - 1) * per_page`. Since you're on page 2 and you're displaying 10 records per page, it skips the first 10 records (from the first page) and starts from the 11th record.

This SQL query is automatically constructed by Kaminari when you call the page and per methods on your `ActiveRecord` model, making it very easy to implement efficient pagination without manually crafting complex SQL queries.

## Quiz

- Why is pagination important in web applications?
- It enhances security by limiting access to data.
  - Not quite. Pagination is primarily about performance and manageability, not security.
- It improves the quality of the data.
  - Not quite. Pagination affects the presentation of data, not its quality.
- It helps improve performance by loading a manageable number of records at a time.
  - Correct! Pagination improves performance and user experience by breaking data into smaller chunks.
{: .choose_best #importance_of_pagination title="Importance of Pagination" points="1" answer="3" }

- What does the `OFFSET` clause do in the context of pagination?
- It limits the number of records returned by the query.
  - Not quite. The `LIMIT` clause handles the number of records, while `OFFSET` skips records.
- It skips the records of previous pages, starting from the record specified by the offset value.
  - Correct! The OFFSET clause skips records based on the current page and per-page limit.
- It sorts the records in ascending order.
  - Not quite. Sorting is handled by the `ORDER BY` clause, not `OFFSET`.
{: .choose_best #sql_offset title="Understanding the OFFSET Clause" points="1" answer="2" }

## Conclusion

You've learned why pagination is critical in applications with large datasets and how to implement it using Kaminari. Additionally, you integrated Bootstrap for styling the pagination controls and gotten a look at how the underlying SQL is generated.

## Resources
- [Kaminari](https://github.com/kaminari/kaminari)
- [will_paginate](https://github.com/mislav/will_paginate)
- [pagy](https://github.com/ddnexus/pagy)
