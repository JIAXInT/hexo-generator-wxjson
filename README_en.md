<!--
 * @Description: English Document
 * @Author: Justic
 * @Date: 2024-11-21 09:32:40
 * @github: https://github.com/JIAXInT
 * @LastEditTime: 2024-11-21 09:32:40
 * @LastEditors: Justic
-->
<div align="right">
  Language:
  English
  <a title="Chinese" href="/README.md">Chinese</a>
</div>

# hexo-generator-wxjson

- Modified based on [hexo-generator-restful](https://www.npmjs.com/package/hexo-generator-restful), added the generation of specified article lists (used for the display of carousel images).
- Based on [hexo-generator-search](https://www.npmjs.com/package/hexo-generator-search), modifications have been made to add aliases for article files, facilitating the configuration of mini-program interfaces.
- Modified based on [hexo-generator-wxapi](https://github.com/ryanuo/hexo-generator-wxapi), added the function of pinning and sorting articles.

## Installation

```bash
npm install hexo-generator-wxjson --save
```

## Configure the `_config.yml` file under hexo

- Add the following as the default configuration. A property value of `false` indicates that it will not be generated.

```yml
restful_api:
  # The "site" can be configured as an array to selectively generate certain properties.
  # site: ['title', 'subtitle', 'description', 'author', 'since', 'email', 'favicon', 'avatar']
  # site: true        # hexo.config mix theme.config
  posts_size: 10 # Article list pagination, 0 means no pagination.
  posts_props: # Properties that need to be generated for article list items.
    title: true
    slug: true
    date: true
    updated: true
    comments: true
    path: true
    excerpt: false
    cover: true # Cover image, taking the first image of the article.
    content: false
    keywords: false
    categories: true
    tags: true
  categories: true # Category data.
  use_category_slug: false # Use slug for the filename of category data.
  tags: true # Tag data.
  use_tag_slug: false # Use slug for the filename of tag data.
  post: true # Article data.
  pages: false # Additional Hexo page data, such as About.
  swipers_list: [] # Generate specified page information. Fill in the names of your article folders, such as ['css', 'js'], without the suffix. Mainly used for the API of carousel images.
  search_all:
    enable: true # Enabled by default.
    path: api/search.json
    field: post
    content: true
```

## Documentation

- `Domain` is the domain address where you deploy, e.g., `https://u.mr90.top` or `https://u.mr90.top/blog`.

| Request Method | Request URL                          | Request Details                                                                                                                                                                                                                                                                 |
| -------------- | ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Get            | Domain + `/api/site.json`            | Get all Hexo configurations (both site configuration and theme configuration).                                                                                                                                                                                                  |
| Get            | Domain + `/api/posts.json`           | If `posts_size: 0` is configured, there will be no pagination, and all articles will be retrieved.                                                                                                                                                                              |
| Get            | Domain + `/api/posts/:PageNum.json`  | Get paginated data. After setting the list classification, `:PageNum` is a dynamic variable (page number), e.g., `/api/1.json`.                                                                                                                                                 |
| Get            | Domain + `/api/tags.json`            | Get all article tags. If there are no tags, nothing will be generated.                                                                                                                                                                                                          |
| Get            | Domain + `/api/tags/:TagName.json`   | Get the list of articles with the specified tag. `:TagName` is the custom tag name of your article, e.g., `/api/tags/web.json`.                                                                                                                                                 |
| Get            | Domain + `/api/categories.json`      | Get all the classifications of articles.                                                                                                                                                                                                                                        |
| Get            | Domain + `/api/categories/:CateName` | Get the list of articles in the specified classification.                                                                                                                                                                                                                       |
| Get            | Domain + `/api/articles/:Slug.json`  | Get the detailed data of an article according to its alias. `:Slug` is the alias of the article.                                                                                                                                                                                |
| Get            | Domain + `/api/swiper.json`          | Get the list of articles with the specified alias. For example, the characters in the array `['web', 'hexo', 'java']` are the aliases of the specified articles. This function is mainly used for the dynamic configuration of carousel image articles in WeChat mini-programs. |
| Get            | Domain + `/api/search.json`          | Get all documents for local global search.                                                                                                                                                                                                                                      |

### Get Implicit Pages

Get the content of Hexo implicit pages from the theme, such as About, etc. Since implicit pages (except for navigation bar entry pages like About) generally do not provide a direct access entry in Hexo, developers who call this API need to know their complete paths. This interface is disabled by default.

For example:

###### Request

```
GET /api/pages/about.json
```
