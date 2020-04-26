# darfig-hugo-theme

custom hugo-theme see [gohugo](https://gohugo.io/)

## Config

**set config.toml:**

Set values:

```toml
baseURL = "."
languageCode = "<lenguageCode>"
title = "<siteTitle>"
theme = "darfig-hugo-theme"


[params]
    UserIcon = "<user icon url>"
    AuthorName = "<name>"
    GitHubuser = "<git user>"
```

### Tree

├── content

│   ├── about.md


│   ├── recommended.md


│   ├── projPreviewImage.jpg/svg/png

│   ├── posts

│   │   ├── post1.md

│   │   ├── post2.md

│   │   └── post3.md

│   └── projects

│       └── project.md

#### Set image project preview 

Put **image: "projPreviewImage.svg"** in the markdown project file. And set image under content/ folder.

#### recommended sites

Create a content/recommended.md file with:

```
---
title: "recommended sites"
type: "recommended"
---

- ![webtitle](url1)
- ![webtitle](url2)

```


## Theme

#### Color Palette 

![palette](./palette.jpg)

#### Screenshots

![general](./general.jpg)

![general1](./general1.jpg)

![posts](./posts.jpg)

![small](./small.jpg)


### Example

See on my [personal website](https://darfig.github.io/)
