# Routing using react-router Part 2

- API Calls
  - Using fetch()
- Third-Party Packages
  - react-loader-spinner


1.Make Api call using fetch()
2.Set Blog data to state
3.show loader while fetching data

making Api call => componentDidMount() step(1)

class BlogsList extends Component {

  componentDidMount() {
    this.getBlogsData()
  
  }
  
  getBlogsData = async() =>{
    const response = await fetch('url') // step 2 add fetch
    const data = await response.json // step 3 getting json data
   
  }
  render() {
    return (
      <div className="blog-list-container">
        {blogsData.map(item => (
          <BlogItem blogData={item} key={item.id} />
        ))}
      </div>
    )
  }
}

export default BlogsList

step 4 converting snake case to camel case 

getBlogsData = async() =>{
    const response = await fetch('https://apis.ccbp.in/blogs')
    const data = await response.json
    const updatingData = data.map((each) =>({
      id : each.id,
      imageUrl : each.image_url,
      title : each.title,
      avatarUrl : each.avatar_url,
      author : each.author,
      topic : each.topic,
    }))
   
  }


  
class BlogsList extends Component {
state = {
  blogsData : [], //setting state step(5)
}

  componentDidMount() {
    this.getBlogsData()
  
  }
  
  getBlogsData = async() =>{
    const response = await fetch('https://apis.ccbp.in/blogs')
    const data = await response.json()
    const updatingData = data.map((each) =>({
      id : each.id,
      imageUrl : each.image_url,
      title : each.title,
      avatarUrl : each.avatar_url,
      author : each.author,
      topic : each.topic,
    }))
    this.setState({blogsData : updatingData}) // upadating setstate (6)
  }
  render() {
    const {blogsData} = this.state // accessing state step(7)
    return (
      <div className="blog-list-container">
        {blogsData.map(item => (
          <BlogItem blogData={item} key={item.id} />
        ))}
      </div>
    )
  }
}

export default BlogsList


installing react spinner:
npm install react-loader-spinner@5.3.4 --legacy-peer-deps
router npm install react-router-dom@5

importing react load spinner:
import Loader from 'react-loader-spinner'
import 'react-loader-spinner/dist/loader/css/react-spinner-loader.css' //step(8)

initialize state step(9):
state = {
  blogsData : [],
  isLoading : true,
}

adding loader (10):
 render() {
    const {blogsData , isLoading} = this.state
    return (
      <div className="blog-list-container">
      {isLoading ? 
      <Loader type ='TailSpin' color='#00BFF' height={50} width={50}/>:
      blogsData.map(item => 
          <BlogItem blogData={item} key={item.id} />
        )} // adding loader
       
      </div>
    )
  }

add setState step(11):
getBlogsData = async () => {
    const response = await fetch('https://apis.ccbp.in/blogs')
    const data = await response.json()
    const formattedData = data.map(eachItem => ({
      id: eachItem.id,
      title: eachItem.title,
      imageUrl: eachItem.image_url,
      avatarUrl: eachItem.avatar_url,
      author: eachItem.author,
      topic: eachItem.topic,
    }))
    this.setState({blogsData: formattedData, isLoading: false})
  }


  React routing part-3

navigating to specific blog => Using path parameters
Path parameters => match

Navigating to specific blog

When we click blog item we are navigating to new route corresponding the path
 /blogs/:id
 steps:
 1. add link for blog item
 2.add blog item details route
 3.make api call for specific details


 add link for blog item step(1):

 import {Link} from 'react-router-dom'
import './index.css'

const BlogItem = props => {
  const {blogData} = props
  const {id, imageUrl, topic, title, avatarUrl, author} = blogData
  return (
    <Link className='blog-item-link' to = {`/blogs/${id}`}> // adding link and template litreatuls
    <div className="item-container">
      <img className="item-image" src={imageUrl} alt={`item${id}`} />

      <div className="item-info">
        <p className="item-topic">{topic}</p>

        <p className="item-title">{title}</p>
        <div className="author-info">
          <img className="avatar" src={avatarUrl} alt={`avatar${id}`} />
          <p className="author-name">{author}</p>
        </div>
      </div>
    </div>
    </Link>
  )
}

export default BlogItem


adding Blog item details route step(2): syntax
<Route path = '/.../:params' component = {component}/>

import {BrowserRouter, Route, Switch} from 'react-router-dom'

import Header from './components/Header'
import About from './components/About'
import Contact from './components/Contact'
import BlogsList from './components/BlogsList'
import NotFound from './components/NotFound'
import BlogItemDetails from './components/BlogItemDetails' // import BlogItemDetails

import './App.css'

const App = () => (
  <BrowserRouter>
    <Header />
    <Switch>
      <Route exact path="/" component={BlogsList} />
      <Route exact path="/about" component={About} />
      <Route exact path="/contact" component={Contact} />
      <Route exact path = "/blogs/:id" component = {BlogItemDetails}/> // adding route
      <Route component={NotFound} />
    </Switch>
  </BrowserRouter>
)

export default App


make api call for specific details step(3):
accessing the path parameters :
When a component is render by the route, some additional props are passed
1.match
2.location
3.history

Match :
the match object contains the information about the path from which the component is rendered

accessing id : BlogItemDetails.js step(4)
 componentDidMount() {
    this.getBlogItemData()
  }
  getBlogItemData = () => {
    const {match} = this.props
    const {params} = match
    const {id} = params
    console.log(id); //accessing id
    
  }

Making HTTP Request step(5)
 getBlogItemData = async() => {
    const {match} = this.props
    const {params} = match
    const {id} = params
    const response = await fetch(`https://apis.ccbp.in/blogs/${id}`)
    const data = await response.json()

    const updatedData ={ // converting snake case to camel case step(6)
      title : data.title,
      imageUrl : data.image_url,
      content : data.content,
      avatarUrl : data.avatar_url,
      author : data.author,

    }
   
setting state step(7)
   class BlogItemDetails extends Component {
  state = {
    blogData : [],
  }
  componentDidMount() {
    this.getBlogItemData()
  }
  getBlogItemData = async() => {
    const {match} = this.props
    const {params} = match
    const {id} = params
    const response = await fetch(`https://apis.ccbp.in/blogs/${id}`)
    const data = await response.json()

    const updatedData ={
      id : data.id,
      title : data.title,
      imageUrl : data.image_url,
      content : data.content,
      avatarUrl : data.avatar_url,
      author : data.author,
      topic : data.topic,

    }
   
    this.setState({blogData : updatedData})
  }
    
  }

  using blog data step(8):

   renderBlogItemDetails = () => {
    const {blogData} = this.state //using
    const {title, imageUrl, content, avatarUrl, author} = blogData
    
    return (
      <div className="blog-info">
        <h2 className="blog-details-title">{title}</h2>

        <div className="author-details">
          <img className="author-pic" src={avatarUrl} alt={author} />
          <p className="details-author-name">{author}</p>
        </div>

        <img className="blog-image" src={imageUrl} alt={title} />
        <p className="blog-content">{content}</p>
      </div>
    )
  }

  render() {
    return <div className="blog-container">{this.renderBlogItemDetails()}</div>
  }
}

export default BlogItemDetails