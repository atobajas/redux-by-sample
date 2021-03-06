# 06 Simple App

This sample series takes as starting point _02 Change Name_

In this sample we will create a minimal application, that will include
a login page, a list like and a detail page. In this sample we will
create the project structure plus the login page functinallity.

We will be using react router redux, and in a later sample we will check
how this integrates with redux dev tools.

We will create the app following an ellaborated structure, this could be an
overkill if we just want to implement such simple app, but the goal is to
simulate a complex scenario, a mid size app.


Summary steps:

- First let's make some cleanup.
- We will continue by installing react-router-redux.
- Let's create basic pages and navigation.
- Let's create a loginEntity, and a userProfile entity.
- Let's add a fake API to simulate a login (_loginApi.tsx_).
- Let's implement our login functionallity.
- Time to move to members lists (Model, API, Action, Reducer, ...).
- Time to move to details views.
- Let's add validations to details view.



# Prerequisites

Install [Node.js and npm](https://nodejs.org/en/) (v6.6.0) if they are not already installed on your computer.

> Verify that you are running at least node v6.x.x and npm 3.x.x by running `node -v` and `npm -v` in a terminal/console window. Older versions may produce errors.

## Steps to build it

- Copy the content from _02 Change Name_ and execute _npm install_.

- Let's make some cleanup:
    - Remove the _./src/helloworld.tsx_ and _./src/helloworldContainer.tsx_.
    - Remove the _./src/nameEdit.tsx_ and _./src/nameEditContainer.tsx_.
    - Remove the _./actions/updateUserProfileName.tsx plus _./actions_ folder.

- It's time to install routing libraries:

```
npm install react-router react-router-redux --save
```

```
npm install @types/react-router @types/react-router-redux --save
```

- Let's install support for promises:

```
npm install core-js --save
```

```
npm install @types/core-js --save
```

- Let's install Redux-Thunk to handle async actions

```
npm install redux-thunk --save
```

```
npm install @types/redux-thunk --save-dev
```

- Let's configure redux-thunk in _main.tsx_

```javascript
import { createStore, applyMiddleware } from 'redux';
//(...)
import reduxThunk from 'redux-thunk';

let store = createStore(
  reducers,
  applyMiddleware(reduxThunk)
);
```


- Let's start working with the pages structure, create the following folder _./src/pages_

- Under pages let's create a subfolder called _./src/pages/login_.

- Let's create under _./src/pages/login/login.tsx_

```javascript
import * as React from 'react';

export const LoginComponent = () => {
  return (
    <h2>Im the login page</h2>
  )
}
```

- Let's create under _./src/pages/login/loginContainer.tsx_

```javascript
import { connect } from 'react-redux';
import { LoginComponent } from './login';

const mapStateToProps = (state) => {
    return {
    }
}

const mapDispatchToProps = (dispatch) => {
  return {
  }
}

export const LoginContainer = connect(
                                   mapStateToProps
                                  ,mapDispatchToProps
                                )(loginComponent);
```


- Let's create under _./src/pages/login/index.tsx_

```javascript
import { connect } from 'react-redux';
import { LoginComponent } from './login';

const mapStateToProps = (state) => {
    return {
    }
}

const mapDispatchToProps = (dispatch) => {
  return {
  }
}

export const LoginContainer = connect(
                                   mapStateToProps
                                  ,mapDispatchToProps
                                )(loginComponent);
```

- Let's follow the same steps to create under _./src/pages/student-list
the folowing files:

studentList.tsx

```javascript
import * as React from 'react';

export const StudentListComponent = () => {
  return (
    <h2>I'm the StudentList page</h2>
  )
}
```

studentListContainer.tsx

```javascript
import { connect } from 'react-redux';
import { StudentListComponent } from './studentList';

const mapStateToProps = (state) => {
    return {
    }
}

const mapDispatchToProps = (dispatch) => {
  return {
  }
}

export const StudentListContainer = connect(
                                   mapStateToProps
                                  ,mapDispatchToProps
                                )(StudentListComponent);
```


index.tsx

```javascript
import {StudentListContainer} from './studentListContainer';

export {
  StudentListContainer
}
```

- Let's follow the same steps to create under _./src/pages/student-detail
the folowing files:

studentDetail.tsx

```javascript
```

studentDetailContainer.tsx

```javascript
import { connect } from 'react-redux';
import { StudentDetailComponent } from './studentDetail';

const mapStateToProps = (state) => {
    return {
    }
}

const mapDispatchToProps = (dispatch) => {
  return {
  }
}

export const StudentDetailContainer = connect(
                                   mapStateToProps
                                  ,mapDispatchToProps
                                )(StudentDetailComponent);
```


index.tsx

```javascript
import {StudentDetailContainer} from './studentDetailContainer';

export {
  StudentDetailContainer
}
```

- Is time to wire up the navigation, let's start by adding _routerReducre_
into ./reducers/index.ts

```
import { combineReducers } from 'redux';
import { userProfileReducer } from './userProfile';
import { Provider } from 'react-redux';
import { routerReducer } from 'react-router-redux'

export const reducers =  combineReducers({
  userProfileReducer,
  routing: routerReducer
});
```

- Let's move to _./src/main.tsx_ and add the routing support (pending to separate
  this routing in a separate file).

  ```javascript
  import * as React from 'react';
  import * as ReactDOM from 'react-dom';
  import { Router, Route, IndexRoute, hashHistory } from 'react-router';
  import { syncHistoryWithStore} from 'react-router-redux'
  import { createStore } from 'redux';
  import { Provider } from 'react-redux';
  import {reducers} from './reducers'
  import {App} from './app';
  import {LoginContainer} from './pages/login';
  import {StudentListContainer} from './pages/student-list';
  import {StudentDetailContainer} from './pages/student-detail';

  let store = createStore(reducers);
  const history = syncHistoryWithStore(hashHistory, store);

  ReactDOM.render(
     <Provider store={store}>
        <div>
          <Router history={history}>
            <Route path="/" component={App}>
              <IndexRoute component={LoginContainer}/>
              <Route path="login" component={LoginContainer}/>
              <Route path="student-list" component={StudentListContainer}/>
              <Route path="student-detail" component={StudentDetailContainer}/>
            </Route>
          </Router>
        </div>
     </Provider>
    , document.getElementById('root'));
  ```

- Time to update _app.tsx_ to place the page container.

```javascript
import * as React from 'react'
import { Link } from 'react-router'

export const App = (props: { children? }) => {
  return (
    <div>
      <header>
        Links:
        {' '}
        <Link to="/">Login</Link>
        {' '}
        <Link to="/student-list">Student List</Link>
        {' '}
        <Link to="/student-detail">Student Detail</Link>
      </header>
      <div style={{ marginTop: '1.5em' }}>{props.children}</div>
    </div>
  )
}
```

- Let's create a loginEntity, under _./src/model_

```javascript
export class LoginEntity {
  login : string;
  password : string;

  public constructor() {
    this.login = '';
    this.password = '';
  }
}
```

- And a userProfile entity.

```javascript
export class UserProfile {
  fullname : string;
  role : string;
}
```

- A loginResponse:

```javascript
import {UserProfile} from './userProfile';

export class LoginResponse {
  succeeded : boolean;
  userProfile : UserProfile;
}
```

- Let's add a fake API to simulate a login (_./src/rest-api/loginApi.tsx_).

```javascript
import {LoginEntity} from '../model/login';
import {UserProfile} from '../model/userProfile';
import {LoginResponse} from '../model/loginResponse';
import {} from 'core-js'

class LoginApi {
  login(loginInfo : LoginEntity) : Promise<LoginResponse> {
      let loginResponse = new LoginResponse();

      if(loginInfo.login === 'admin' && loginInfo.password === 'test') {
        loginResponse.succeeded = true;
        loginResponse.userProfile = {fullname: "John Doe", role: 'admin' };
      } else {
        loginResponse.succeeded = false;
        loginResponse = null;
      }

      return Promise.resolve(loginResponse);
  }
}
```

- Let's get started implementing our login functionallity, first we will define
a perform login action (_./src/common/actions_):

```javascript
export const actionsEnums = {
  USERPROFILE_PERFORM_LOGIN : 'UPDATE_USERPROFILE_NAME '
}
```

- Login action will be asynchronous (we need to break it into two actions and use
  redux-thunk), we will create two actions _loginRequestStarted_ and _loginRequestCompleted_.

- Since this action is something we will fire only on the
login window we will keep this under the following path _./src/pages/login/actions/loginRequestStarted.ts_

```javascript
import {actionsEnums} from '../../../common/actionsEnums';
import {LoginEntity} from '../../../model/login';
import {loginApi} from '../../../rest-api/loginApi';
import {loginRequestCompletedAction} from './loginRequestCompleted';
import { hashHistory } from 'react-router';

export const loginRequestStartedAction = (login : LoginEntity) => {
  return function(dispatcher) {
    const promise = loginApi.login(login);

    promise.then(
      data => {
        dispatcher(loginRequestCompletedAction(data));

        // This is not ideal to have it here, maybe move it to middleware?
        if(data.succeeded == true) {
          hashHistory.push('/student-list');
        }
      }

    );

    return promise;
  }
}```

- Now the completed _./src/pages/login/actions/loginRequestCompleted.ts_

```javascript
import {actionsEnums} from '../../../common/actionsEnums';
import {LoginResponse} from '../../../model/loginResponse';

export const loginRequestCompleted = (loginResponse : LoginResponse) => {
  return {
    type: actionsEnums.USERPROFILE_PERFORM_LOGIN,
    payload: loginResponse
  }
}
```

- On the reducers side, let's remove the _./src/reducers/userProfile.ts_ reducer
and add a new reducer that we will call _./src/reducers/session.ts

```javascript
import {actionsEnums} from '../common/actionsEnums';
import objectAssign = require('object-assign');
import {UserProfile} from '../model/userProfile';
import {LoginResponse} from '../model/loginResponse';
import {LoginEntity} from '../model/login';

class SessionState  {
  isUserLoggedIn : boolean;
  userProfile : UserProfile;
  editingLogin : LoginEntity;

  public constructor()
  {
    this.isUserLoggedIn = false;
    this.userProfile = new UserProfile();
  }
}

export const sessionReducer =  (state : SessionState = new SessionState(), action) => {
      switch (action.type) {
        case actionsEnums.USERPROFILE_PERFORM_LOGIN:
           return handlePerformLogin(state, action.payload);


      }

      return state;
};


const handlePerformLogin = (state : SessionState, payload : LoginResponse) => {
  const newState = objectAssign({}, state, {isUserLoggedIn: payload.succeeded, userProfile: payload.userProfile});
  return newState;
}
```

- Let's register this reducer _./src/reducers/index.ts_:

```javascript
import { combineReducers } from 'redux';
import { sessionReducer } from './session';
import { Provider } from 'react-redux';
import { routerReducer } from 'react-router-redux'

export const reducers =  combineReducers({
  sessionReducer,
  routing: routerReducer
});
```

- It's time to jump into the ui part, we will use the login layout created in
a previous sample, from repo [React By Sample: login form](https://github.com/Lemoncode/react-by-sample/tree/master/15%20LoginForm) .
We will create the following files:

_./src/login/components/header.tsx_

```javascript
import * as React from "react"

export const Header = () => {
  return (    
  	   <div className="panel-heading">
  	     <h3 className="panel-title">Please sign in</h3>
  	   </div>
  );
}
```

_./src/login/components/form.tsx_

```javascript
import * as React from "react"
import {hashHistory} from 'react-router'
import {LoginEntity} from '../../../model/login';

interface Props {
   loginInfo : LoginEntity;
   updateLoginInfo : (loginInfo : LoginEntity) => void;
   performLogin : () => void;
}

export const Form = (props: Props) => {
  return (
    <div className="panel-body">
      <form role="form">
        <fieldset>
          <div className="form-group">
      		  <input className="form-control" placeholder="E-mail" name="email" type="text"
              value={props.loginInfo.login}
              onChange={(e : any) => props.updateLoginInfo({login: e.target.value, password: props.loginInfo.password })}
            />
      		</div>
          <div className="form-group">
            <input className="form-control" placeholder="Password" name="password" type="password"
              value={props.loginInfo.password}
              onChange={(e : any) => props.updateLoginInfo({login: props.loginInfo.login, password: e.target.value })}
            />
          </div>
          <input className="btn btn-lg btn-success btn-block" value="Login"
            onClick={(e) => {props.performLogin()}}
          />
        </fieldset>
      </form>
    </div>
  );
}
```

- Before continuing with the UI we have realized that this form components emits an event with the current
editing login information (by doing this we can easily initialize it), we have to add to the session state
a new property to hold this information, and an action to set it.

_./src/common/actionEnums.ts_

```javascript
export const actionsEnums = {
  USERPROFILE_UPDATE_EDITING_LOGIN:  'USERPROFILE_UPDATE_EDITING_LOGIN',
  USERPROFILE_PERFORM_LOGIN : 'USERPROFILE_PERFORM_LOGIN'
}
```

_./src/pages/login/actions/updateEditingLogin.ts

```javascript
import {actionsEnums} from '../../../common/actionsEnums';
import {LoginEntity} from '../../../model/login';

export const loginAction = (loginInfo : LoginEntity) => {
  return {
    type: actionsEnums.USERPROFILE_UPDATE_EDITING_LOGIN,
    payload: loginInfo
  }
}
```

_./src/reducers/session.ts_

```javascript
import {actionsEnums} from '../common/actionsEnums';
import objectAssign = require('object-assign');
import {UserProfile} from '../model/userProfile';
import {LoginResponse} from '../model/loginResponse';
import {LoginEntity} from '../model/login';

class SessionState  {
  isUserLoggedIn : boolean;
  userProfile : UserProfile;
  editingLogin : LoginEntity;

  public constructor()
  {
    this.isUserLoggedIn = false;
    this.userProfile = new UserProfile();
    this.editingLogin = new LoginEntity();
  }
}

export const sessionReducer =  (state : SessionState = new SessionState(), action) => {
      switch (action.type) {
        case actionsEnums.USERPROFILE_PERFORM_LOGIN:
           return handlePerformLogin(state, action.payload);

        case actionsEnums.USERPROFILE_UPDATE_EDITING_LOGIN:
           return handleUpdateEditingLogin(state, action.payload);
      }

      return state;
};


const handlePerformLogin = (state : SessionState, payload : LoginResponse) => {
  const newState = objectAssign({}, state, {isUserLoggedIn: payload.succeeded, userProfile: payload.userProfile});
  return newState;
}

const handleUpdateEditingLogin = (state: SessionState, payload : LoginEntity) => {
  const newState = objectAssign({}, state, {editingLogin: payload});
  return newState;
}
```

- It's time to build the layout of the login Page _./src/pages/login/login.tsx_

```javascript
import * as React from 'react';
import {Header} from './components/header';
import {Form} from './components/form';
import {LoginEntity} from '../../model/login';

interface Props {
   loginInfo : LoginEntity;
   updateLoginInfo : (loginInfo : LoginEntity) => void;
   performLogin : () => void;
}

export const LoginComponent = (props : Props) => {
  return (
    <div className="container">
      <div className="row">
        <div className="col-md-4 col-md-offset-4">
          <div className="panel panel-default">
            <Header/>
            <Form loginInfo={this.props.loginInfo}
                  updateLoginInfo={this.props.updateLoginInfo.bind(this)}
                  performLogin={this.props.performLogin.bind(this)}
                  />
          </div>
        </div>
      </div>
    </div>
  )
}
```

- No we can wire up the loginContainer component with all the reducers info and actions
_./src/pages/login/loginContainer.tsx_

```

```




- ** Separate sample
- Time to move to members lists (Model, API, Action, Reducer, ...).
- Time to move to details views.
- Let's add validations to details view.
