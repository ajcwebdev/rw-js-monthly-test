# Redwood

> **WARNING:** RedwoodJS software has not reached a stable version 1.0 and should not be considered suitable for production use. In the "make it work; make it right; make it fast" paradigm, Redwood is in the later stages of the "make it work" phase.

## Getting Started
- [Tutorial](https://redwoodjs.com/tutorial/welcome-to-redwood): getting started and complete overview guide.
- [Docs](https://redwoodjs.com/docs/introduction): using the Redwood Router, handling assets and files, list of command-line tools, and more.
- [Redwood Community](https://community.redwoodjs.com): get help, share tips and tricks, and collaborate on everything about RedwoodJS.

### Setup

We use Yarn as our package manager. To get the dependencies installed, just do this in the root directory:

```terminal
yarn install
```

### Fire it up

```terminal
yarn redwood dev
```

Your browser should open automatically to `http://localhost:8910` to see the web app. Lambda functions run on `http://localhost:8911` and are also proxied to `http://localhost:8910/.redwood/functions/*`.

## Create a Redwood app and open code editor

```
yarn create redwood-app ./rw-js-monthly-test
cd rw-js-monthly-test
code .
```

## Initialize git repo with main branch, add all files to staging, and commit project

```
git init -b main && git add . && git commit -m "Initial commit"
```

## Set remote and push to origin

```
git remote add origin https://github.com/ajcwebdev/rw-js-monthly-test.git
git push -u origin main
```

## Start development server

```
yarn rw dev
```

## Create pages and layout

```
yarn rw g page home /
yarn rw g page about
yarn rw g layout blog
```

## BlogLayout

```js
// web/src/layouts/BlogLayout/BlogLayout.js

import { Link, routes } from '@redwoodjs/router'

const BlogLayout = ({ children }) => {
  return (
    <>
      <header>
        <h1>
          <Link to={routes.home()}>Redwood Blog</Link>
        </h1>
        <nav>
          <ul>
            <li>
              <Link to={routes.about()}>About</Link>
            </li>
          </ul>
        </nav>
      </header>
      <main>{children}</main>
    </>
  )
}

export default BlogLayout
```

## HomePage

```js
// web/src/pages/HomePage/HomePage.js

import BlogLayout from 'src/layouts/BlogLayout'

const HomePage = () => {
  return <BlogLayout>Home</BlogLayout>
}

export default HomePage
```

## AboutPage

```js
// web/src/pages/AboutPage/AboutPage.js

import BlogLayout from 'src/layouts/BlogLayout'

const AboutPage = () => {
  return (
    <BlogLayout>
      <p>
        This site was created to demonstrate my mastery of Redwood: Look on my
        works, ye mighty, and despair!
      </p>
    </BlogLayout>
  )
}

export default AboutPage
```

## Prisma Schema

```prisma
model Post {
  id        Int      @id @default(autoincrement())
  title     String
  body      String
  createdAt DateTime @default(now())
}
```

## Save and apply migration

```
yarn redwood db save "create posts"
yarn rw db up
```

## Scaffold CRUD interface for posts and create cell to display blog posts

```
yarn rw g scaffold post
yarn rw g cell BlogPosts
```

## BlogPostsCell

```js
// web/src/components/BlogPostsCell/BlogPostsCell.js

export const QUERY = gql`
  query BlogPostsQuery {
    posts {
      id
      title
      body
      createdAt
    }
  }
`

export const Loading = () => <div>Loading...</div>

export const Empty = () => <div>Empty</div>

export const Failure = ({ error }) => <div>Error: {error.message}</div>

export const Success = ({ posts }) => {
  return JSON.stringify(posts)
}
```

```js
// web/src/pages/HomePage/HomePage.js

import BlogLayout from 'src/layouts/BlogLayout'
import BlogPostsCell from 'src/components/BlogPostsCell'

const HomePage = () => {
  return (
    <BlogLayout>
      <BlogPostsCell />
    </BlogLayout>
  )
}

export default HomePage
```

## Success

```js
export const Success = ({ posts }) => {
  return posts.map((post) => (
    <article key={post.id}>
      <header>
        <h2>{post.title}</h2>
      </header>
      <p>{post.body}</p>
      <div>Posted at: {post.createdAt}</div>
    </article>
  ))
}
```

## Setup Netlify Deployment

```
yarn rw g deploy netlify
```

## Set provider to postgres for production

```
datasource DS {
  provider = "postgres"
  url      = env("DATABASE_URL")
}
```

## Commit final state of project

```
git add .
git commit -m "Deploy and database setup"
git push -u origin
```

## Set connection pool

```
postgres://<user>:<pass>@<url>/<db>?connection_limit=1
```