<pre>
In this first code journal I'm going to take a look at my contract sample where I worked these first days.  
In particolar I'm going to comment the state.rs and the msg.rs files:  


//In this first 5 line we are importing our external crates.  
use schemars::JsonSchema;             //  this allow us to generate Json Schema files.  
use serde::{Deserialize, Serialize};  //  this two allow structs to be serialized and deserialized.  

use cosmwasm_std::Addr;               //  we are importing the "Addr" struct (it's simple a String where we store address).  
use cw_storage_plus::Item;            //  allow us to store items.  

// We are defining our "State" struct which is public and contain a i32 type and a Addr struct.  
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]  //we are using the attribute "derive" and other macro.  
pub struct State {    
    pub count: i32,    
    pub owner: Addr,    
}                     
 
pub const STATE: Item<State> = Item::new("state");  // Here we store our state  
  
  
  
Now the msg.rs file:

use cosmwasm_std::Coin; // we are importing a Coin struct which contain a String and a Uint128 type  
use schemars::JsonSchema;  
use serde::{Deserialize, Serialize}  

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]  
#[serde(rename_all = "snake_case")] //[serde(rename_all = "snake_case")] //rename the following struct with snake case convention  
pub struct InstantiateMsg {  
    pub count: i32,         // we are passing the parameter to instantiate the contract state  
}  

// The ExecuteMsg enum take care of the message for the execute function
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum ExecuteMsg {
    Increment {},
    DecrementBy {amount: i32},
    IncrementBy {amount: i32},
    Reset {},
    SendFunds{address: String, token: Coin}
}

// The following enum cover the variants for our query messages
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    HasReset {}, 
}

// This struct is used to create a custom response message but it's not implemented in our code yet
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub struct CustomResponse {
    val: String,
}

//We use MigrateMsg when we are migrating the contract but it's not implemented in our code yet 
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum MigrateMsg {}


Now it's time to answer the following question:
a. What are the concepts?
In this 2 files we can find different type (like structs, enums), the use of some attribute (derive, serde) and some macros (Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)

b. What is the organization?
The code is organaized because each files do only a thing like in the msg.rs we'll only find the message option, in the state only the state struct

c. What is the contract doing? What is the mechanism?
This contract do some simply entry point like instantiate a message, execute a messages and do some queries.



</pre>
