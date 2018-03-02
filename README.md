# gettingstarted
Documentation for React Developers

Bappo doesnt have any help or documentation yet, but for developers who are helping us to build apps on Bappo, here is a quick start guide to learn the basics:

# First App
Go to https://go.bappo.com
Sign up and create your first app
Once you're inside the app, create an object and call it "Tasks"
It is important that you call it "Tasks" otherwise the code examples wont work later on.
Now click on Add New Field
Name the field: "Status"
Field Type Should be: Fixed List
Enter two items in the list
1. New
2. Done


The app is now ready to be used, click on "Go to this App" in the bottom left.
Enter some tasks.

If everything works fine, you can now start coding.

Click on "Modify This App" in the bottom left.
The panel on the left is called the design panel;

Now Click on the Advanced Design Icon at the bottom of the Design Panel.

Click on Coding.

Click on Create Package and call it "example1".

Paste the following code and save:
```javascript
// this example demonstrates basic api calls to 
// - read all records of one table
// - create one new record
// - delete one record by Id

import React from 'react';
import { styled } from 'bappo-components';

class Tasks extends React.Component {
  state = {
    tasks: [],
    newTask: '',
    loading: false,
  }

  fetchData = async () => {
    const { Task } = this.props.$models; 
    const tasks = await Task.findAll();
    this.setState({
      tasks,
    })
  }

  addTask = async () => {
    const { Task } = this.props.$models; 
    const newTask = { name: this.state.newTask };
    const task = await Task.create(newTask);
    this.setState({ tasks: [ ...this.state.tasks, task ], newTask: '' });
  }

  removeTask = async (task) => {
    const { Task } = this.props.$models; 
    const tasks = this.state.tasks.filter(t => t !== task);
    this.setState({ tasks });
    await Task.destroy({where: {id: task.id}});
  }


  componentDidMount() {
    this.fetchData();
  }

  render() {
    return <Container>
      <div>
        {this.state.tasks.map( t => <Card>
        <span>{t.name}</span>
        <BottomLink onClick={() => this.removeTask(t)}> delete </BottomLink>        
        </Card>)}
      </div>
      <Card>
      <input class="newtask" 
        ref={input => input && input.focus()}
        placeholder="new task" 
        name='new' 
        onChange={e => this.setState({ newTask: e.target.value })}
        value={this.state.newTask}
      />
      { this.state.newTask !== '' &&
        <BottomLink onClick={this.addTask}> add </BottomLink>
      }
      </Card>
    </Container>
  }
}

export default Tasks;

const Container = styled.div`
  margin: 20px;
  overflow-y: scroll;
`;

const Card = styled.div`
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  color: gray;
  padding: 0px 20px;
  width: 200px;
  height: 100px;
  float: left;
  margin: 20px;
  border-top: 1px solid #eee;
  border-left: 1px solid #eee;
  box-shadow: 6px 6px 6px rgba(0,0,0,0.2);
  .newtask {
    border: none;
    border-bottom: 1px solid #eee;    
    outline: none;
  }
`;

const BottomLink = styled.div`
  position: absolute;
  bottom: 5px;
  right: 5px;
  font-size: 10px;
  cursor: pointer;
  &:hover{
    color: black;
  }
`;
```





