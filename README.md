# React-Router-6.4

##
https://www.youtube.com/watch?v=L2kzUg6IzxM&t=32s
##
https://github.com/academind/react-router-6.4-intro
##

Api.js 
export async function getPosts() {
  const response = await fetch('https://jsonplaceholder.typicode.com/posts');
  if (!response.ok) {
    throw { message: 'Failed to fetch posts.', status: 500 };
  }
  return response.json();
}

export async function getPost(id) {
  const response = await fetch(
    'https://jsonplaceholder.typicode.com/posts/' + id
  );
  if (!response.ok) {
    throw { message: 'Failed to fetch post.', status: 500 };
  }
  return response.json();
}

export async function savePost(post) {
  if (post.title.trim().length < 5 || post.body.trim().length < 10) {
    throw { message: 'Invalid input data provided.', status: 422 };
  }

  const response = await fetch('https://jsonplaceholder.typicode.com/posts', {
    method: 'POST',
    body: JSON.stringify(post),
    headers: {
      'Content-Type': 'application/json',
    },
  });

  if (!response.ok) {
    throw { message: 'Could not save post.', status: 500 };
  }
}

##
App.jsx
import { RouterProvider, createBrowserRouter, createRoutesFromElements, Route, Routes } from 'react-router-dom';

import BlogLayout from './pages/BlogLayout';
import BlogPostsPage, { loader as blogdPostsLoader } from './pages/BlogPosts';
import NewPostPage, { actions as newPostAction} from './pages/NewPost';
import PostDetailPage, {loader as blogPostLoader } from './pages/PostDetail';
import RootLayout from './pages/RootLayout';
import WelcomePage from './pages/Welcome';

const router = createBrowserRouter( createRoutesFromElements(<Route element={<RootLayout/>}>
  <Route path="/" element={<RootLayout />} errorElement={< ErrorPage;/>}
  <Route index element={<WelcomePage />}>
    <Route path="/blog" element={<BlogLayout />}> 
    ,<Route index element = {<BlogPostsPage/>} loader={blogdPostsLoader}/>
    <Route 
    path =":id" 
    element={<PostDetailPage />}
    loader={blogPostLoader}
    />
  </Route>
  <Route 
  path="/blog/new" 
  element={<NewPostPage />} 
  action={newPostAction}/>
</Route>));

function App() {
  return (
    <RouterProvider router={router}/>
 
  );
}

##
export default App;


NewPost.jsx
import { useNavigate, useNavigation } from 'react-router-dom';
import NewPostForm from '../components/NewPostForm';
import { savePost } from '../util/api';

// function NewPostPage() {
//   const data = useActionData(); 
//   const navigate = useNavigate();
     const navigation = useNavigation();
  

//   function cancelHandler() {
//     navigate('/blog');
//   }

//   return ( 
//     <>
         { data && data <p>{data.message}</p>}
//       <NewPostForm
//         onCancel={cancelHandler}
//         submitting={navigation.state === 'submitting'}
//       />
//     </>
//   );
}

export default NewPostPage;

export async function action({request}) {
  const formData = await request.formData();
  const post={
    title: formData.get('title'),
    body: formData.get('post-text')
  }
  try{
    await savePost(post);
  } catch (err) {
    if(err.status=== 422) {
     return err;
    }
    throw err;
  }
   return redirect ('/blog'); 
  
}

##
BlogPosts.jsx
import { useLoaderData } from 'react-router-dom';
import Posts from '../components/Posts';
import { getPosts } from '../util/api';

function BlogPostsPage() {
  userLoaderData(); 
  const LoaderData = useLoaderData();
  
  return (
    <>
      <h1>Our Blog Posts</h1>
      <Posts blogPosts={posts} />}
    </>
  );
}

export default BlogPostsPage;

export function loader () {
  return getPosts();
}
export default PostDetailPage;

export function loader({params}) {
  const postId = params.id;
  return getPost(postId)
}

##
PostDetail.jsx
  import { useLoaderData } from 'react-router-dom';

  import BlogPost from '../components/BlogPost';
  import { getPost } from '../util/api';

  function PostDetailPage() {
    cost postData = useLoaderData();

    return (
      <>
        
        <BlogPost title={postData.title} text={postData.body} />}
      </>
    );
  }

  export default PostDetailPage;

  export function loader({params}) {
    const postId = params.id;
    return getPost(postId)
    
 ##   
    RootLayout.jsx
    import { Outlet } from 'react-router-dom';
import MainNavigation from '../components/MainNavigation';

function RootLayout() {
  return (
    <>
      <MainNavigation />
      <main>{Out let}</main>
    </>
  );
}

export default RootLayout;


