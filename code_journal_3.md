<pre>

For our code journey 3 we are going to take a look at contract.rs where we call our functions and define the entry_point


#[cfg(not(feature = "library"))] // we provide a conditional compilation 
use cosmwasm_std::entry_point;
use cosmwasm_std::{Binary, Deps, DepsMut, Env, MessageInfo, Response, StdResult, to_binary};

use crate::error::ContractError;
use crate::msg::{ExecuteMsg, InstantiateMsg, QueryMsg};
use crate::state::{State, STATE};
use crate::execute::{*}; //we import our execute functions
use crate::query::{*}; //we import our query functions


// the next to lines are used when you need to migrate a contract (not our case)
//const CONTRACT_NAME: &str = "crates.io:test1";
//const CONTRACT_VERSION: &str = env!("CARGO_PKG_VERSION");

//In the following function we instantiate the state 
#[cfg_attr(not(feature = "library"), entry_point)] //we add the entry_point feauture
pub fn instantiate(
    deps: DepsMut,
    _env: Env,
    info: MessageInfo,
    msg: InstantiateMsg,
) -> Result<Response, ContractError> {
    let state = State {
        count: msg.count, // we set the count with the value in the InstantiateMsg struct
        owner: info.sender.clone(), // we clone the sender in owner
    };

    STATE.save(deps.storage,&state); // we save the state (here we borrow the state to the save function)

    Ok(Response::new() //we hadd same attribute response
        .add_attribute("method", "instantiate")
        .add_attribute("owner", info.sender)
        .add_attribute("count", msg.count.to_string())) // we need to convert the count (i32) to a string
    
}

//In this function we call all the execute function for every ExecuteMsg variant
#[cfg_attr(not(feature = "library"), entry_point)]
pub fn execute(
    deps: DepsMut,
    _env: Env,
    info: MessageInfo,
    msg: ExecuteMsg,
) -> Result<Response, ContractError> {
    match msg { // we do an exhaustive match for the ExecuteMsg enum
        ExecuteMsg::Increment {} => increment_by(deps,1), // we increment the state.count by 1
        ExecuteMsg::IncrementBy {amount} => increment_by(deps, amount), // we increment the state.count by an amount
        ExecuteMsg::DecrementBy {amount} => decrement_by(deps, amount), // we decrement the state.count by an amount
        ExecuteMsg::Reset {} => reset(deps, info), // we reset the state.count to 0
        ExecuteMsg::SendFunds{address, token} => send_funds(deps, address, token), // we send some funds using the BankMsg type from cosmos_msg library
    }
}

// Here is where we call function for our queries  
#[cfg_attr(not(feature = "library"), entry_point)]
pub fn query(deps: Deps, _env: Env, msg: QueryMsg) -> StdResult<Binary> { // query need a binary result 
    match msg {
        QueryMsg::HasReset {} => to_binary(&has_reset(deps)?), // we check if the state is resetted
    }
}


</pre>
