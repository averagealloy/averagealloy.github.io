---
layout: post
title:      "What goes into a button in your React app"
date:       2020-03-03 10:51:41 -0500
permalink:  what_goes_into_a_button_in_your_react_app
---


# Setting the stage 
A little set up goes along way. So with this application, It was made with create-react-app. After that there are four folders actions, components, containers and finally reducers. Now there is some stuff between the button we are looking at in this blog and how we got to that button prior but for this blog I am just going to go through a single button.


# What button am I talking about 
There is one button inparticlular I am talking about. It is the delete button for a specfic transaction that is related to an account. Before we get into the nitty gritty I want to talk about what that button does in plain, easily understandable english. So when your in your account you can see the current balance of that account and the name at the top. after that there is a form  to either make a withdraw or deposit with a given amount you want to transact and then a submit button but at the moment we dont care about that button. After that we we return the account container and the transaction container. It looks like this.

```
import React from 'react'
import AccountEdit from './AccountEdit'


import TransactionsContainer from '../containers/TransactionsContainer'

const Account = (props) => {

    let account = props.accounts[props.match.params.id - 1]
    
return (
    <div>

         <h2>
            {account ? account.name : null} - {account ? account.balance : null}
        </h2>
        <TransactionsContainer account={account}/>
        <AccountEdit account={account}/>
    </div>
)


}

export default Account
```


# What now? 

After looking at that snippit we now know that we should start looking at that transaction container in the hunt for our delete button. So here is what the transactions container looks like. 
```
import React from 'react'
import TransactionInput from '../components/TransactionInput'
import Transactions from '../components/Transactions'

class TransactionsContainer extends React.Component {

    render() {
        return (
            <div>
                <TransactionInput account={this.props.account}/>
                <Transactions transactions={this.props.account && this.props.account.transactions}/>
            </div>
        )
    }

}

export default TransactionsContainer
```

from whats inside the snippet it looks like we have two options 

1.  we could check transactions input component
2. or we could takle a look at the the transactions component 


# Let's dive into our options 

So for the first option transaction input, by the name alone it tips us off that this is maybe not the best place to look for our delete button. To aid in this hypothesis, looking at the snippet above we are giving the transaction input account and account has `this.props.account`. From that I can say that transactionsInput needs account for adding the balance and making the right calculations but to be tripple sure here is the transactions input component.

```
import React from 'react'
import {connect} from 'react-redux'
import {addTransaction} from '../actions/addTransaction'

class TransactionInput extends React.Component {

    state = {
        kind: 'deposit',
        amount: ''
    }


    handleChange = (event) => {
        this.setState({
            [event.target.name]: event.target.value
        })
    }

    handleSubmit = (event) => {
        event.preventDefault()
        this.props.addTransaction(this.state, this.props.account.id)
        this.setState({
            kind: 'deposit',
            amount: ''
        })
    }

    render(){
        return(
            <div>
                <form onSubmit={this.handleSubmit}>
                    <label>Transaction Type:</label>
                    <select className='ui dropdown' name='kind' value={this.state.kind} onChange={this.handleChange}>
                        <option value='1'>deposit</option>
                        <option value='2'>withdraw</option>
                    </select>
                        <label>Transaction Amount:</label>
                        <div className='ui input'>
                            <input type='text' name='amount' value={this.state.amount} onChange={this.handleChange}/>
                        </div>
                    <input  className='ui primary button' type='submit'/>
                </form>
            </div>
        )
    }
}

export default connect(null, {addTransaction}) (TransactionInput)

```
looking at the component we have our inputs and exports followed by state defined with an object that contains kind and amount then to functions one to handle change an another to handle submit. After that we see our form that has our drop down, input, and submit button so now we 10000000% know that we can cross off option 1 from the list.


# The almighty second option 
Flipping back to the transactions container (for reference)


```
import React from 'react'
import TransactionInput from '../components/TransactionInput'
import Transactions from '../components/Transactions'

class TransactionsContainer extends React.Component {

    render() {
        return (
            <div>
                <TransactionInput account={this.props.account}/>
                <Transactions transactions={this.props.account && this.props.account.transactions}/>
            </div>
        )
    }

}

export default TransactionsContainer
```

We are now looking at the transacion component and what it's being passed in. This time just like transaction input we are passing a varible this time it is transactions. transactions is equal to `this.props.account && this.props.account.transactions` so we are getting all of the account info(i.e name of the account and current balance) then we are also grabing all of the transactions for that specfic account.

# We have found what we are looking for.

Here is what the component looks like then we will dive into it a bit more.

```
import React from 'react'
import {connect} from 'react-redux'
import {deleteTransaction} from '../actions/deleteTransaction'
import {Link} from 'react-router-dom'


const Transactions = (props) => {

const handleDelete = (transaction) => {
    
    props.deleteTransaction(transaction.id, transaction.account_id)
}

    return(
        <div>
            {props.transactions && props.transactions.map( transaction => 
                <ul key={transaction.id}>
                    {transaction.kind} - {transaction.amount} <br/>
                    <button className='ui primary button' onClick={() => handleDelete(transaction)}>
                        Delete
                    </button> 
                </ul>
            )}

             <Link to={'/accounts'}>
                <button className='ui primary button'>Back to All Accounts</button> 
             </Link>

        </div>
    )
}

export default connect(null, {deleteTransaction})(Transactions)
```



we have our usual suspects our imports and exports. Then we take a look at some variables that are defined
one is called Transactions. Transactions returns an object. That object houses a definition and a return statement.

# Jump around

The definition inside of the transaction object refers to file in the actions. Here is that file.
```
export const deleteTransaction = (transactionId, accountId) => {

    return dispatch => {
        return fetch(`http://localhost:3001/api/v1/accounts/${accountId}/transactions/${transactionId}`, {
            method: 'DELETE'
        })
        .then(response => response.json())
        .then(account => dispatch({type: 'DELETE_TRANSACTION', payload: account}))
    }

}

```

What on earth is this? This is telling a case statement in the reducer if delete transaction is called then do this. Before we look at the reducer lets look at this. the first part, go to the api, make sure your at the right version of the api, got to accounts, go to the account that has the right transactions your looking for, go to transactions, and finally go to the right transaction that you want. After you are then you want to specfiy what your looking for in this case delete. Then once the fetch call is made the 'then' key word is used saying "great that you have finished my request please return a json object, O , and by the way if you could delete the transaction and return the account that would be great". 

Lets take a look at the delete transaction case in the reducer.(this is not the whole file but a snippet from it.)

```
 case 'DELETE_TRANSACTION':
           let accountsDelete = state.accounts.map(account => {
               if(account.id === action.payload.id) {
                   return action.payload
               } else {
                   return account
               }
           })
```


Here is another instance of what on earth is that, every ones favorite game.  But this one is a bit more manageable though. Its a case statement with a definition.  The definition is saying map over the state of the accounts and takes in an arugument. The arugument is saying that account points to an arrow function that arrow function contains out if statement. The if statement is saying if the account id matched the payload id then return the payload if not return the original account with the previous transactions. 

# Conclusion

That is how a kind of button works in a React/Redux application. Now if your are like me your probably wondering if the juice is worth the squeeze. In a larger application this library makes more sense for for a large application I'm not sure about it for a small application. I don't want this to detract from the amazing work that Facebook has done and keep doing. I hope that this has helped in demonstrating what goes into a simple button in a React/Redux application.

