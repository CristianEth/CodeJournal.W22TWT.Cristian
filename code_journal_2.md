<pre>
In the second code journal I'm going to comment the execute.rs where we find most of the functions of this contract:

use crate::state::{STATE}; // we import our STATE item
use crate::error::ContractError; // we import our ContractError enum where we define our errors variants
use cosmwasm_std::{DepsMut, MessageInfo, Response, BankMsg, SubMsg, Coin}; // we import from the cosmwasm_std library some usefull types, structs or function

// In the increment_by function we are essentially going to increment the count from the state by an "amount"
pub fn increment_by (deps: DepsMut, amount: i32) -> Result<Response, ContractError> { // we return an Result type which allow a <succes, failure> 
    STATE.update(deps.storage, |mut state| -> Result<_, ContractError> { //we use the update function to change the count
        state.count += amount; //where we increment the count from the state
        Ok(state) // we return state as the success value
    });

    // in the next 3 lines we add 2 attribute for the response message to explain what's done
    Ok(Response::new()
    .add_attribute("action", "increment")
    .add_attribute("amount", amount.to_string())) //the amount incremented
}

// we do de same as increment but the difference is that we decrement by an "amount"
pub fn decrement_by(deps: DepsMut, amount: i32) -> Result<Response, ContractError>{
    STATE.update(deps.storage, |mut state | -> Result<_, ContractError> {
        state.count -= amount;
        if  state.count-amount < 0{ 
            return Err(ContractError::InvalidDecrement{}) // we return failure if the count-amount is <= 0 
        }
        Ok(state) 
        
    });

    Ok(Response::new()
    .add_attribute("action", "decrementBy")
    .add_attribute("amount", amount.to_string())) //the amount decremented
}

// Here we simply reset our state.count
pub fn reset(deps: DepsMut, info: MessageInfo) -> Result<Response, ContractError> {
    STATE.update(deps.storage, |mut state| -> Result<_, ContractError> {
        if info.sender != state.owner {      //Only the owner can do the reset action 
            return Err(ContractError::Unauthorazied{})
        }
        state.count = 0; // reset the count
        Ok(state) 
    }); 
    Ok(Response::new()
    .add_attribute("action", "reset state"))   
}

pub fn send_funds(deps: DepsMut, address: String, token: Coin) -> Result<Response, ContractError> {
    let verified_addr = deps.api.addr_validate(&address)?; // we check if the recipient is a valid address
    let send_msg: SubMsg = SubMsg::new(BankMsg::Send{to_address: verified_addr.to_string(), amount: vec![token]}); // then we crate the Sub Message

    Ok(Response::new().add_submessage(send_msg)) // we add a sub messagge to our response
}



Now the answer:
a. What are the concepts?
In addition to the code journal 1 we find the Result type which allow a Success or a Failure, how to to add attribute to a response and how to use a sub message.

b. What is the organization?
We can see that here (in the execution.rs file) we find only the execute function.

c. How could it be better? More efficient? Safer? 
This code could be improved by adding also an error variant to our functions.


</pre>
