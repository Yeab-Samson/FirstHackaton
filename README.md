# @zemenay/blog-system

A complete, modular blog system for Next.js applications with Supabase backend integration. Built for the **Zemenay Tech Solutions Hackathon**.

## ⚡ Super Quick Start (30 seconds)

```bash
# 1. Clone and install
git clone https://github.com/zemenay-tech/blog-system
cd blog-system
npm install

# 2. Set up environment (create .env.local)
echo "NEXT_PUBLIC_SUPABASE_URL=your_supabase_url" > .env.local
echo "NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_key" >> .env.local

# 3. Run the demo
npm run dev

# 4. Visit http://localhost:3000/blog to see it working!
```

## 🚀 Installation as NPM Package

### Installation

```bash
npm install @zemenay/blog-system
```

### Basic Setup

1. **Install dependencies** in your Next.js project:
```bash
npm install @supabase/supabase-js next react react-dom
```

2. **Set up environment variables** in `.env.local`:
```env
NEXT_PUBLIC_SUPABASE_URL=your_supabase_project_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
```

3. **Initialize the blog system** in your Next.js app:
```tsx
// pages/_app.tsx
import { createBlogConfigFromEnv } from '@zemenay/blog-system';

// Initialize blog system
createBlogConfigFromEnv();

export default function MyApp({ Component, pageProps }) {
  return <Component {...pageProps} />;
}
```

4. **Create blog pages** using the provided components:
```tsx
// pages/blog/index.tsx
import { BlogList, useBlogPosts } from '@zemenay/blog-system';

export default function BlogPage() {
  const { posts, loading } = useBlogPosts();
  
  return (
    <div>
      <h1>Our Blog</h1>
      <BlogList posts={posts} loading={loading} />
    </div>
  );
}
```

## 📋 Requirements Met

✅ **Next.js Frontend**: Provides React components optimized for Next.js  
✅ **Modular & Installable**: Published as npm package `@zemenay/blog-system`  
✅ **Production Ready**: Type-safe, tested, and optimized  
✅ **Minimal Setup**: Works with just environment variables  
✅ **Supabase Integration**: Full PostgreSQL backend via Supabase  
✅ **Admin Panel**: Complete content management system  
✅ **Integration Tests**: Comprehensive test suite included  

## 🏗️ Architecture

### Core Modules

- **Components**: Ready-to-use React components for blog functionality
- **Hooks**: Custom hooks for data fetching and state management  
- **API Layer**: Type-safe API client for Supabase operations
- **Types**: Complete TypeScript definitions
- **Utils**: Markdown parsing, slug generation, and more

### Database Schema

The system creates these Supabase tables:
- `blog_posts` - Main blog content with metadata
- `blog_categories` - Post categorization
- Row Level Security enabled for data protection

## 📦 Components

### BlogList
Display a list of blog posts with search and filtering.

```tsx
import { BlogList } from '@zemenay/blog-system';

<BlogList 
  posts={posts}
  loading={loading}
  showExcerpt={true}
  showCategory={true}
  baseUrl="/blog"
/>
```

### BlogPost  
Render individual blog posts with markdown support.

```tsx
import { BlogPost } from '@zemenay/blog-system';

<BlogPost 
  post={post}
  showMeta={true}
/>
```

### AdminDashboard
Complete admin interface for managing blog posts.

```tsx
import { AdminDashboard } from '@zemenay/blog-system';

<AdminDashboard
  onDeletePost={handleDelete}
  editPostUrl={(id) => `/admin/posts/${id}/edit`}
  newPostUrl="/admin/posts/new"
/>
```

### BlogEditor
Rich markdown editor for creating and editing posts.

```tsx
import { BlogEditor } from '@zemenay/blog-system';

<BlogEditor
  post={existingPost}
  onSave={handleSave}
  onCancel={handleCancel}
/>
```

## 🎣 Hooks

### useBlogPosts
Fetch and manage blog posts with filtering.

```tsx
import { useBlogPosts } from '@zemenay/blog-system';

const { posts, loading, error } = useBlogPosts({
  query: 'search term',
  category: 'Technology',
  limit: 10
});
```

### useBlogPost
Fetch a single blog post by slug.

```tsx
import { useBlogPost } from '@zemenay/blog-system';

const { post, loading, error } = useBlogPost('post-slug', true);
```

### useAdminPosts
Admin hook for managing all posts.

```tsx
import { useAdminPosts } from '@zemenay/blog-system';

const { posts, loading, refetch, user } = useAdminPosts();
```

## 🔧 API Usage

### Direct API Access
For custom implementations, use the BlogAPI directly:

```tsx
import { BlogAPI, useBlog } from '@zemenay/blog-system';

const { createPost, updatePost, deletePost } = useBlog();

// Create new post
const newPost = await createPost({
  title: 'My New Post',
  content: '# Hello World\n\nThis is my first post!',
  published: true
}, userId);

// Update existing post
await updatePost({
  id: 'post-id',
  title: 'Updated Title',
  content: 'Updated content'
});
```

## 🎨 Styling

The components use Tailwind CSS classes and are designed to integrate seamlessly with your existing design system. Key features:

- **Responsive Design**: Mobile-first approach
- **Customizable**: Override styles with custom CSS
- **Accessible**: WCAG compliant components
- **Dark Mode Ready**: CSS variables for theming

### Custom Styling
```tsx
<BlogList 
  className="my-custom-blog-list"
  posts={posts}
/>
```

## 🗄️ Database Setup

### Supabase Configuration

1. **Create a new Supabase project**
2. **Run the database schema** (provided by the package):

```sql
-- The package provides the complete schema
-- Run this in your Supabase SQL editor
-- (Schema is logged when you initialize the system)
```

3. **Set up Row Level Security policies** (auto-generated)
4. **Configure authentication** as needed

### Schema Overview

```typescript
interface BlogPost {
  id: string;
  title: string;
  content: string;          // Markdown content
  excerpt?: string;         // SEO excerpt
  category?: string;        // Post category
  published: boolean;       // Published status
  views: number;           // View count
  created_at: string;      // ISO timestamp
  updated_at: string;      // ISO timestamp
  author_id?: string;      // User ID from auth.users
  slug: string;            // URL-friendly slug
  meta_title?: string;     // SEO title
  meta_description?: string; // SEO description
  featured_image?: string; // Image URL
  tags?: string[];         // Post tags
}
```

## 🔐 Authentication

The system integrates with Supabase Auth for user management:

```tsx
import { getCurrentUser, signIn, signOut } from '@zemenay/blog-system';

// Get current user
const user = await getCurrentUser();

// Sign in
await signIn('user@example.com', 'password');

// Sign out
await signOut();
```

## 🧪 Testing

### Running Tests

```bash
npm test
```

### Integration Tests
The package includes comprehensive integration tests:

```typescript
import { setupTestEnvironment, createMockPost } from '@zemenay/blog-system/tests';

describe('My Blog Integration', () => {
  beforeEach(() => {
    setupTestEnvironment();
  });

  test('should display blog posts', () => {
    const mockPost = createMockPost({ title: 'Test Post' });
    // Your test code here
  });
});
```

## 📱 Example Pages

### Blog Index Page
```tsx
// pages/blog/index.tsx
import { BlogList, useBlogPosts } from '@zemenay/blog-system';

export default function BlogIndex() {
  const { posts, loading } = useBlogPosts({ limit: 10 });
  
  return (
    <div className="max-w-6xl mx-auto px-4 py-8">
      <h1 className="text-3xl font-bold mb-8">Our Blog</h1>
      <BlogList posts={posts} loading={loading} />
    </div>
  );
}
```

### Individual Post Page
```tsx
// pages/blog/[slug].tsx
import { BlogPost, useBlogPost } from '@zemenay/blog-system';
import { useRouter } from 'next/router';

export default function PostPage() {
  const router = useRouter();
  const { slug } = router.query;
  const { post, loading } = useBlogPost(slug as string, true);
  
  if (loading) return <div>Loading...</div>;
  if (!post) return <div>Post not found</div>;
  
  return (
    <div className="max-w-4xl mx-auto px-4 py-8">
      <BlogPost post={post} />
    </div>
  );
}
```

### Admin Dashboard
```tsx
// pages/admin/blog.tsx
import { AdminDashboard } from '@zemenay/blog-system';

export default function AdminBlog() {
  return (
    <div className="min-h-screen bg-gray-50">
      <div className="max-w-7xl mx-auto px-4 py-8">
        <AdminDashboard />
      </div>
    </div>
  );
}
```

## 🔧 Configuration

### Advanced Configuration
```typescript
import { createBlogConfig } from '@zemenay/blog-system';

createBlogConfig({
  supabaseUrl: 'your-url',
  supabaseAnonKey: 'your-key',
  adminRole: 'admin',           // Default: 'admin'
  postsPerPage: 15,            // Default: 10
  enableComments: true,        // Default: false
  enableCategories: true,      // Default: true
  enableTags: true            // Default: true
});
```

### Environment Variables
```env
# Required
NEXT_PUBLIC_SUPABASE_URL=your_supabase_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key

# Optional
BLOG_ADMIN_ROLE=admin
BLOG_POSTS_PER_PAGE=10
BLOG_ENABLE_COMMENTS=false
BLOG_ENABLE_CATEGORIES=true
BLOG_ENABLE_TAGS=true
```

## 🚀 Deployment

### Vercel (Recommended)
1. Push to GitHub
2. Connect to Vercel
3. Add environment variables
4. Deploy!

### Other Platforms
The system works with any Next.js hosting platform:
- Netlify
- Railway  
- DigitalOcean App Platform
- AWS Amplify

## 📝 Migration Guide

### From Other Blog Systems
```typescript
// Example migration from existing blog data
import { BlogAPI } from '@zemenay/blog-system';

const api = new BlogAPI();

// Migrate existing posts
const existingPosts = await getExistingPosts();
for (const post of existingPosts) {
  await api.createPost({
    title: post.title,
    content: post.content,
    published: post.published,
    slug: generateSlug(post.title)
  }, authorId);
}
```

## 🤝 Contributing

This package was built for the Zemenay Tech Solutions Hackathon.

### Development Setup
```bash
git clone https://github.com/zemenay-tech/blog-system
cd blog-system
npm install
npm run dev
```

### Building
```bash
npm run build
```

### Publishing
```bash
npm publish
```

## 📄 License

MIT License - See [LICENSE](LICENSE) file for details.

## 🏆 Hackathon Submission

This modular blog system demonstrates:

✅ **Complete Integration**: Seamlessly plugs into any Next.js application  
✅ **Production Ready**: Type-safe, tested, and optimized for performance  
✅ **Modern Stack**: Next.js + Supabase + TypeScript + Tailwind CSS  
✅ **Developer Experience**: Minimal setup, excellent documentation  
✅ **Scalability**: Built for production workloads  
✅ **Security**: Row Level Security and proper authentication  
✅ **Testing**: Comprehensive integration test suite  
✅ **Documentation**: Complete examples and API reference  

### Repository Transfer
Ready for ownership transfer to Zemenay Tech Solutions as requested.

---

**Built with ❤️ for the Zemenay Tech Solutions Hackathon**

For support, please open an issue on [GitHub](https://github.com/zemenay-tech/blog-system).
