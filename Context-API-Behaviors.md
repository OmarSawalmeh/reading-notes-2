# **Context API - Behaviors**

## **Globally Accessible**
The state of our snackbars (which ones are visible) can be localized to a single centralized component. That same centralized component can be responsible for rendering them. There’s really no need for another component somewhere in the tree to hook into that state (at least not in our use-case).

However, the management of that state (adding, removing) needs to be globally accessible.

Context can let us do just this.

```js
export const SnackBarContext = createContext()

export function SnackBarProvider({ children }) {
  const [alerts, setAlerts] = useState([])

  return (
    <SnackBarContext.Provider value={{ setAlerts }}>
      {children}
      {alerts.map((alert) => <SnackBar key={alert}>{alert}</SnackBar>)}
    </SnackBarContext.Provider>
  )
}
```
Our context provider is now responsible for both displaying the local state of the snackbars (we call them alerts), and for exposing an API for globally managing them.

The other half of the puzzle is we now need a consumer for this provider. It turns out that our custom hook from before is nothing more than a small wrapper around the useContext internal React hook, which consumes our new context.

```js
import { useContext } from 'react'

import { SnackBarContext } from 'components/snack-bar-provider'

const useSnackBars = () => useContext(SnackBarContext)

export default useSnackBars
```

This is not perfect yet though. Our provider exposes the state mutator setAlerts directly, but we need something much higher level. We don’t want our components fumbling around with how to add an alert without destroying existing ones, and so on. Think of setAlerts as a private method, and we need to expose a public method that abstracts away the noise.

## **Higher Level API & Behaviour**

```js
export const SnackBarContext = createContext()

const AUTO_DISMISS = 5000

export function SnackBarProvider({ children }) {
  const [alerts, setAlerts] = useState([])
  
  const activeAlertIds = alerts.join(',')
  useEffect(() => {
    if (activeAlertIds.length > 0) {
      const timer = setTimeout(() => setAlerts((alerts) => alerts.slice(0, alerts.length - 1)), AUTO_DISMISS)
      return () => clearTimeout(timer)
    }
  }, [activeAlertIds])

  const addAlert = (alert) => setAlerts((alerts) => [alert, ...alerts])

  const value = { addAlert }
    
  return (
    <SnackBarContext.Provider value={value}>
      {children}
      {alerts.map((alert) => <SnackBar key={alert}>{alert}</SnackBar>)}
    </SnackBarContext.Provider>
  )
}
```


## **Here is an example illustrating how you might inject a “theme” using the new context API:**
```js
const ThemeContext = React.createContext('light');

class ThemeProvider extends React.Component {
  state = {theme: 'light'};

  render() {
    return (
      <ThemeContext.Provider value={this.state.theme}>
        {this.props.children}
      </ThemeContext.Provider>
    );
  }
}

class ThemedButton extends React.Component {
  render() {
    return (
      <ThemeContext.Consumer>
        {theme => <Button theme={theme} />}
      </ThemeContext.Consumer>
    );
  }
}
```
## **React v16.3.0: New lifecycles and context API**
For many years, React has offered an experimental API for context. Although it was a powerful tool, its use was discouraged because of inherent problems in the API, and because we always intended to replace the experimental API with a better one.



## **Official Context API**
For many years, React has offered an experimental API for context. Although it was a powerful tool, its use was discouraged because of inherent problems in the API, and because we always intended to replace the experimental API with a better one.

Version 16.3 introduces a new context API that is more efficient and supports both static type checking and deep updates.

# Contributor Covenant Code of Conduct

## Our Pledge

In the interest of fostering an open and welcoming environment, we as
contributors and maintainers pledge to making participation in our project and
our community a harassment-free experience for everyone, regardless of age, body
size, disability, ethnicity, gender identity and expression, level of experience,
nationality, personal appearance, race, religion, or sexual identity and
orientation.

## Our Standards

Examples of behavior that contributes to creating a positive environment
include:

* Using welcoming and inclusive language
* Being respectful of differing viewpoints and experiences
* Gracefully accepting constructive criticism
* Focusing on what is best for the community
* Showing empathy towards other community members

Examples of unacceptable behavior by participants include:

* The use of sexualized language or imagery and unwelcome sexual attention or
advances
* Trolling, insulting/derogatory comments, and personal or political attacks
* Public or private harassment
* Publishing others' private information, such as a physical or electronic
  address, without explicit permission
* Other conduct which could reasonably be considered inappropriate in a
  professional setting

## Our Responsibilities

Project maintainers are responsible for clarifying the standards of acceptable
behavior and are expected to take appropriate and fair corrective action in
response to any instances of unacceptable behavior.

Project maintainers have the right and responsibility to remove, edit, or
reject comments, commits, code, wiki edits, issues, and other contributions
that are not aligned to this Code of Conduct, or to ban temporarily or
permanently any contributor for other behaviors that they deem inappropriate,
threatening, offensive, or harmful.

## Scope

This Code of Conduct applies both within project spaces and in public spaces
when an individual is representing the project or its community. Examples of
representing a project or community include using an official project e-mail
address, posting via an official social media account, or acting as an appointed
representative at an online or offline event. Representation of a project may be
further defined and clarified by project maintainers.


---

[Reading Page](./README.md)

---

