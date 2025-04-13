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