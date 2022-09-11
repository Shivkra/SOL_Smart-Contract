# SOL_Smart-Contract

## Get local Solana env running.
### Choose your path
Getting Solana setup all starts with your machine. There are a bunch of "gotchyas" on different OS's. If you are running an Intel macOS machine or Linux machine feel free to move right on through. If you are running a Windows machine or M1 macOS machine follow one of the links below:

Setup Solana on Windows Machine: https://github.com/buildspace/buildspace-projects/blob/main/Solana_And_Web3/en/Section_2/Resources/windows_setup.md

Setup Solana on a M1 macOS Machine: https://github.com/buildspace/buildspace-projects/blob/main/Solana_And_Web3/en/Section_2/Resources/m1_setup.md

### Install Rust
In Solana, programs are written in Rust! If you don't know Rust don't worry. As long as you know some other language — you'll pick it up over the course of this project.

To install Rust, you can just follow the installation steps here. There are clear steps for getting Rust installed for Windows, Linux, and Mac.

Once you're done, verify by doing: 

'rustup --version'
- Then, make sure the rust compiler is installed:

'rustc --version'
- Last, let's make sure Cargo is working as well. Cargo is the rust package manager.

'cargo --version'
- As long as all those commands output a version and didn't error, you're good to go!

### Install Solana
Solana has a super nice CLI that's going to be helpful later when we want to test the programs we write.

Again, the installation steps are pretty straight forward here. There are clear steps for getting the Solana CLI installed for Windows, Linux, and Mac.

Don't worry about upgrading to the latest version of Solana.

Note: Depending on your system — once you install Solana, it may output a message like "Please update your PATH environment variable" and it'll give you a line to copy and run. Go ahead and copy + run that command so your PATH gets setup properly.

Once you're done installing, run this to make sure stuff is working:

'solana --version'
- If that output a version number, you're good to go!

Next thing you'll want to do is run these two commands separately:

'solana config set --url localhost'
'solana config get'

This will output something like:

Config File: /Users/flynn/.config/solana/cli/config.yml
RPC URL: http://localhost:8899
WebSocket URL: ws://localhost:8900/ (computed)
Keypair Path: /Users/flynn/.config/solana/id.json 
Commitment: confirmed
- This means that Solana is set up to talk to our local network! When developing programs, we're going to be working w/ our local Solana network so we can quickly test stuff on our computer.

The last thing to test is we want to make sure we can get a local Solana node running. Basically, remember how we said that the Solana chain is run by "validators"? Well — we can actually set up a validator on our computer to test our programs with.

'solana-test-validator'

###Notes for Windows users
If you are a Windows user and the above command doesn't work, or you get the following error Unable to connect to validator: Client error: test-ledger/admin.rpc does not exist make sure you do the following.

Open WSL instead of Powershell.
Enter the command 'cd ~/' to get out of the starting directory
Now enter 'solana-test-validator'

Boom!! You're now running a local validator. Pretty cool :).

If you are running an Intel Mac and see the error below you will need to install the OpenSSL library. The easiest way to do this would be through brew like so: 'brew install openssl@1.1'

solana-gif-portal solana-test-validator
dyld: Library not loaded: /usr/local/opt/openssl@1.1/lib/libssl.1.1.dylib
  Referenced from: /Users/<your-username>/.local/share/solana/install/active_release/bin/solana-test-validator
  Reason: image not found
Now, go ahead and CONTROL + C to stop the validator. We're never going to actually use 'solana-test-validator' manually ourselves again. The workflow we're going to follow will actually automatically run the validator in the background for us. I just wanted to show you it working so you can start getting an idea of how stuff is working magically as we move forward ;).

### Install Node, NPM, and Mocha
Pretty solid chance you already have Node and NPM. When I do node --version I get v16.0.0. The minimum version is v11.0.0. If you don't have node and NPM, get it here.

After that, be sure to install this thing called Mocha. It's a nice little testing framework to help us test our Solana programs.

'npm install -g mocha'
	
### Anchor
We're going to be using this tool called "Anchor" a lot. If you know about Hardhat from the world of Ethereum, it's sorta like that! Except — it's built for Solana. Basically, it makes it really easy for us to run Solana programs locally and deploy them to the actual Solana chain when we're ready!

Installing this thing was a little troublesome for me, but, I got it working via the steps below! We're going to build it from source. Note: If you're on Linux, there are some special instructions you can follow here. Mac and Windows below. Also, if you're using Linux for Windows, follow the Linux commands!

To install Anchor, go ahead an run:

'cargo install --git https://github.com/project-serum/anchor anchor-cli --locked'
- The above command may take a while. Once it's done, it may ask you to update your path, remember to do that.

From here run:

'anchor --version'
- If you got that working, nice, you have Anchor!!

We'll also use Anchor's npm module and Solana Web3 JS — these both will help us connect our web app to our Solana program!

'npm install @project-serum/anchor @solana/web3.js'
	
### Create a test project and run it
Okay, we're nearly done haha. The last thing we need to do to finalize installation is to actually run a Solana program locally and make sure it actually works.

Let's start a boilerplate Solana project named myepicproject.

'anchor init myepicproject --javascript'
'cd myepicproject'
	
###Notes for windows users
1. Run the commands using WSL2 and not powershell.
2. If 'cargo install --git https://github.com/project-serum/anchor avm --locked --force' gives you errors. Refer to the Anchor user documents. You might need to install the Linux (WSL) dependencies. To do this, run 'sudo apt-get update && sudo apt-get upgrade && sudo apt-get install -y pkg-config build-essential libudev-dev'
3. If you get further issues such as error: failed to run custom build command for openssl-sys v0.9.71, run 'sudo apt install libssl-dev'
4. Once these dependencies have been installed, the command from step 2 should work.
5. Now set the anchor version with 'avm use latest' and you should be good to go!
'anchor init' will create a bunch of files/folders for us. It's sorta like 'create-react-app' in a way. We'll check out all the stuff it's created in moment.

If you are running the project locally and don't have yarn installed 'anchor init' will fail. To solve this you can install yarn by running 'npm install --global yarn' .

### Create a local keypair
Next thing we need to do is actually generate a local Solana wallet to work with. Don't worry about creating a passphrase for now, just tap "Enter" when it asks.

'solana-keygen new'
- What this will do is create a local Solana keypair — which is sorta like our local wallet we'll use to talk to our programs via the command line. If you run solana config get you'll see something called Keypair Path. That's where the wallet has been created, feel free to check it out :).

If you run:

'solana address'
- You'll see the public address of your local wallet we just created.

### Let's run our program
When we did 'anchor init' it created a basic Solana program for us. What we want to do now is:

1. Compile our program.
2. Spin up 'solana-test-validator' and deploy the program to our local Solana network w/ our wallet. This is kinda like deploying our local server w/ new code.
3. Actually call functions on our deployed program. This is kinda like hitting a specific route on our server to test that it's working.
Anchor is awesome. It lets us do this all in one step by running:

Note: Be sure you don't have 'solana-test-validator' running anywhere else it'll conflict w/ Anchor. This took me a while to figure out lol.

'anchor test'
- This may take a while the first time you run it! As long as you get the green words at the bottom that say "1 passing" you're good to go!! 

Note: If you receive the message node: --dns-result-order= is not allowed in NODE_OPTIONS this mean you are on an older version of Node and technically, this didn't pass! Since I tested this all with Node v16.13.0 I would highly suggest you just upgrade to this version. Upgrading node is a pain, learn more here. I like using nvm.

Note: If you get this message Error: Your configured rpc port: 8899 is already in use and you do not have application which is listenning to port 8899, try to run solana-test-validator, and in the next terminal tab anchor test --skip-local-validator. It should run fine.

Congrats you've successfully set up your Solana environment :).
	
	
	
## Write your first Solana program.

Go ahead an open up myepicproject in VSCode.

If you are on Windows, remember that this all needs to be done with WSL. In case you don't remember where you installed everything in your Ubuntu instance, follow these steps to get back to your project:

Press 'windows' + R to open up the RUN Box. This is where you can type the command \\wsl$\Ubuntu and an explorer window should pop up.
Inside these folders, go to the home folder and then username folder. This is where you will find myepicproject!

You'll see all the magic stuff Anchor has generated for us here.

Delete the contents of programs/myepicproject/src/lib.rs and  tests/myepicproject.js. Don't actually delete the files, just what's in them.

Note: I didn't actually install the Rust extension for VSCode. It already has Rust syntax highlighting for me out of the box.

### A basic program
Let's write our first Solana program! This Rust code is going to live in the lib.rs file.

Here's what it looks like:

use anchor_lang::prelude::*;
declare_id!("Fg6PaFpoGXkYsidMpWTK6W2BeZ7FEfcYkg476zPFsLnS");
#[program]
pub mod myepicproject {
  use super::*;
  pub fn start_stuff_off(ctx: Context<StartStuffOff>) -> Result <()> {
    Ok(())
  }
}
#[derive(Accounts)]
pub struct StartStuffOff {}
- A lot happening here so let's just step line-by-line. Again, if you don't know Rust — don't worry too much. I think you can pick this stuff up pretty quickly. You won't become a Rust Master like this, but, you can worry about that later :).

use anchor_lang::prelude::*;
- A simple use declaration at the top. Kinda like an import statement. We want to import in a lot of the tools Anchor provides for us to make writing Solana programs easier.

declare_id!("Fg6PaFpoGXkYsidMpWTK6W2BeZ7FEfcYkg476zPFsLnS");
- We'll cover this thing a little later. Basically, this is the "program id" and has info for Solana on how to run our program. Anchor has generated this one for us. We'll be changing it later.

#[program]
- This is how we tell our program, "Hey — everything in this little module below is our program that we want to create handlers for that other people can call". You'll see how this comes into play. But, essentially this lets us actually call our Solana program from our frontend via a fetch request. We'll be seeing this #[blah] syntax a few places.

They're called macros — and they basically attach code to our module. It's sorta like "inheriting" a class.

pub mod myepicproject {
  use super::*;
  pub fn start_stuff_off(ctx: Context<StartStuffOff>) -> Result <()> {
    Ok(())
  }
}
- pub mod tells us that this is a Rust "module" which is an easy way to define a collection of functions and variables — kinda like a class if you know what that is. And we call this module myepicproject. Within here we write a function start_stuff_off which takes something called a Context and outputs a Result <()>. You can see this function doesn't do anything except call Ok(()) which is just a Result type you can read about here.

So really, this thing start_stuff_off is just a function that someone else can call now. It doesn't do anything right now, but, we'll change that :).

#[derive(Accounts)]
pub struct StartStuffOff {}
- Lastly we have this thing at the bottom. It'll be more obvious why this is important later. But basically it's another macro. Here, we'll basically be able to specify different account constraints. Again, it'll make more sense in a bit hehe.

Let's just get stuff running and see what happens.

### Write a script to see it working locally
We need to basically tell Anchor how we want our program to run and what functions we want to call. Head over to tests/myepicproject.js. This is actually written in javascript :).

Go ahead and code this up:

const anchor = require('@project-serum/anchor');
const main = async() => {
  console.log("🚀 Starting test...")
  anchor.setProvider(anchor.AnchorProvider.env());
  const program = anchor.workspace.Myepicproject;
  const tx = await program.rpc.startStuffOff();
  console.log("📝 Your transaction signature", tx);
}
const runMain = async () => {
  try {
    await main();
    process.exit(0);
  } catch (error) {
    console.error(error);
    process.exit(1);
  }
};
runMain();
- We can step line by line here. First off, the runMain thing is just some javascript magic to get our main function to run asynchronously. Nothing special really.

The real magic happens here:

anchor.setProvider(anchor.AnchorProvider.env());
const program = anchor.workspace.Myepicproject;
const tx = await program.rpc.startStuffOff();
- First, we tell Anchor to set our provider. So, it gets this data from solana config get. In this case, it's grabbing our local environment! This way Anchor knows to run our code locally (later we'll be able to test our code on devnet!).

Then, we grab anchor.workspace.Myepicproject and this is a super cool thing given to us by Anchor that will automatically compile our code in lib.rs and get it deployed locally on a local validator. A lot of magic in one line and this is a big reason Anchor is awesome.

Note: Naming + folder structure is mega important here. Ex. Anchor knows to look at programs/myepicproject/src/lib.rs b/c we used anchor.workspace.Myepicproject.

Finally, we actually call our function we made by doing program.rpc.startStuffOff() and we await it which will wait for our local validator to "mine" the instruction.

Before we run it, need to make a quick change.

In Anchor.toml we want to change the [scripts] tags a little:

[scripts]
test = "node tests/myepicproject.js"
- Keep everything else in Anchor.toml the same!

Finally, let's run it using:

anchor test
- Here's what I get near the bottom:

🚀 Starting test...
📝 Your transaction signature 4EPghDAKXjtseY1dB4DT3xwpt18L1QrL8qbAJ3a3mRaTTZURkgBuUhN3sNhppDbwJNRL75fE53ucTBytoPWNEMAx
	
Note: If you are using VSCode, don't forget to save all the files you're changing before running anchor test! I personally ran into so many issues because I thought I saved the file, but in reality I didn't :(.

Note: If you see this error Attempt to load a program that does not exist, you can do solana address -k target/deploy/myepicproject-keypair.json and replace with this address every occurency in lib.rs, Anchor.toml, and  myepicproject.js.

BOOOOM. YOU DID IT.

As long as you see a "transaction signature", you're good! This means you successfully called the startStuffOff function and this signature is basically your receipt.

Pretty epic. You've written a Solana program, deployed it to your local Solana node, and are now actually speaking to your deployed program on your local Solana network.

NICEEEEEEE. I know it may not seem like much but you now have a basic flow to do stuff.

1. Write code in lib.rs
2. Test specific functions using tests/myepicproject.js.
Get used to this cycle! It's the fastest way to iterate on your Solana programs.
	


##⚡️ Store basic data on our contract.

Right now, our program does literally nothing haha. Let's change it up to store some data!

Our website will allow people to submit GIFs. So, storing something like a total_gifs number would be pretty helpful too.

### Create an integer to store GIF count
Cool so we just want to store a basic integer with the number of total_gifs people have submitted. So, every time someone adds a new gif we'd just do total_gifs += 1.

Let's think about this.

Remember earlier I said Solana programs are stateless. They don't hold data permanently. This is very different from the world of Ethereum smart contracts — which hold state right on the contract.

But, Solana programs can interact w/ "accounts".

Again, accounts are basically files that programs can read and write to. The word "accounts" is confusing and super shitty. For example, when you create a wallet on Solana — you create an "account". But, your program can also make an "account" that it can write data to. Programs themselves are considered "accounts".

Everything is an account lol. Just remember an account isn't just like your actual wallet — it's a general way for programs to pass data between each other. Read about them more here.

Check out the code below, I added some comments as well.

use anchor_lang::prelude::*;
declare_id!("Fg6PaFpoGXkYsidMpWTK6W2BeZ7FEfcYkg476zPFsLnS");
#[program]
pub mod myepicproject {
  use super::*;
  pub fn start_stuff_off(ctx: Context<StartStuffOff>) -> Result <()> {
    // Get a reference to the account.
    let base_account = &mut ctx.accounts.base_account;
    // Initialize total_gifs.
    base_account.total_gifs = 0;
    Ok(())
  }
}
// Attach certain variables to the StartStuffOff context.
#[derive(Accounts)]
pub struct StartStuffOff<'info> {
    #[account(init, payer = user, space = 9000)]
    pub base_account: Account<'info, BaseAccount>,
    #[account(mut)]
    pub user: Signer<'info>,
    pub system_program: Program <'info, System>,
}
// Tell Solana what we want to store on this account.
#[account]
pub struct BaseAccount {
    pub total_gifs: u64,
}
- A lot happening here. Let's step through it.

### Initializing an account
Lets check out this line at the bottom:

#[account]
pub struct BaseAccount {
    pub total_gifs: u64,
}
- This is dope. Basically, it tells our program what kind of account it can make and what to hold inside of it. So, here, BaseAccount holds one thing and it's an integer named total_gifs.

Then, here we actually specify how to initialize it and what to hold in our StartStuffOff context.

#[derive(Accounts)]
pub struct StartStuffOff<'info> {
    #[account(init, payer = user, space = 9000)]
    pub base_account: Account<'info, BaseAccount>,
    #[account(mut)]
    pub user: Signer<'info>,
    pub system_program: Program <'info, System>,
}
- Looks complicated.

First we've got [account(init, payer = user, space = 9000)]. All we're doing here is telling Solana how we want to initialize BaseAccount.

Note, if after running your test below you get the error Transaction simulation failed: Error processing Instruction 0: custom program error: 0x64, you will need to change space = 9000 to space = 10000. If you look at these docs from anchor you can see that they define a simple program that declares space = 8 + 8 (eg, 8 kilobytes + 8 kilobytes). The more logic we add to our program, the more space it will take up!

1. init will tell Solana to create a new account owned by our current program.
2. payer = user tells our program who's paying for the account to be created. In this case, it's the user calling the function.
3. We then say space = 9000 which will allocate 9000 bytes of space for our account. You can change this # if you wanted, but, 9000 bytes is enough for the program we'll be building here!
Why are we paying for an account? Well — storing data isn't free! How Solana works is users will pay "rent" on their accounts. You can read more on it here and how rent is calculated. Pretty wild, right? If you don't pay rent, validators will clear the account!

Here's another article from the docs on rent I liked as well!

"With this approach, accounts with two-years worth of rent deposits secured are exempt from network rent charges. By maintaining this minimum-balance, the broader network benefits from reduced liquidity and the account holder can rest assured that their Account::data will be retained for continual access/usage."

We then have pub user: Signer 'info which is data passed into the program that proves to the program that the user calling this program actually owns their wallet account.

Finally, we have pub system_program: Program which is actually pretty freaking cool. It's basically a reference to the SystemProgram. The SystemProgram is the program that basically runs Solana. It is responsible for a lot of stuff, but one of the main things it does is create accounts on Solana. The SystemProgram is a program the creators of Solana deployed that other programs like ours talk to haha — it has an id of 11111111111111111111111111111111.

Lastly, we do this thing in our function where we just grab base_account from the StartStuffOff context by doing Context<StartStuffOff>.

pub fn start_stuff_off(ctx: Context<StartStuffOff>) -> Result <()> {
	// Get a reference to the account.
  let base_account = &mut ctx.accounts.base_account;
	// Initialize total_gifs.
  base_account.total_gifs = 0;
  Ok(())
}
- Boom! Again — a lot of this stuff may seem confusing especially if you're new to Rust but let's just keep writing and running code. I think this stuff makes more sense the more you do write code → run → get errors → fix errors → repeat.

Note: We do &mut to get a "mutable reference" to base_account. When we do this it actually gives us the power to make changes to base_account. Otherwise, we'd simply be working w/ a "local copy" of base_account.

### Retrieve account data
Let's put it all together.

So, we can actually retrieve account data now as well over in javascript land. Go ahead and update myepicproject.js. I added some comments on lines I changed.

const anchor = require('@project-serum/anchor');
// Need the system program, will talk about this soon.
const { SystemProgram } = anchor.web3;
const main = async() => {
  console.log("🚀 Starting test...")
  // Create and set the provider. We set it before but we needed to update it, so that it can communicate with our frontend!
  const provider = anchor.AnchorProvider.env();
  anchor.setProvider(provider);
  const program = anchor.workspace.Myepicproject;
  // Create an account keypair for our program to use.
  const baseAccount = anchor.web3.Keypair.generate();
  // Call start_stuff_off, pass it the params it needs!
  let tx = await program.rpc.startStuffOff({
    accounts: {
      baseAccount: baseAccount.publicKey,
      user: provider.wallet.publicKey,
      systemProgram: SystemProgram.programId,
    },
    signers: [baseAccount],
  });
  console.log("📝 Your transaction signature", tx);
  // Fetch data from the account.
  let account = await program.account.baseAccount.fetch(baseAccount.publicKey);
  console.log('👀 GIF Count', account.totalGifs.toString())
}
const runMain = async () => {
  try {
    await main();
    process.exit(0);
  } catch (error) {
    console.error(error);
    process.exit(1);
  }
};
runMain();
- anchor.web3.Keypair.generate() may also be kinda confusing — why are we doing this? Well, basically it's because we need to create some credentials for the BaseAccount we're creating.

Most of the script is the same but you'll see I pass startStuffOff some important params that we specified in the struct pub struct StartStuffOff.

Note: notice also that in lib.rs the function is called start_stuff_off since in Rust we use snake case (snake_case) instead of camel case. But, over in our javascript file we use camel case and actually call startStuffOff. This is something nice Anchor does for us so we can follow best practices regardless of what language we're using. You can use snake case in Rust-land and camel case in JS-land.

And perhaps the coolest part about all this is where we call:

await program.account.baseAccount.fetch(baseAccount.publicKey)
console.log('👀 GIF Count', account.totalGifs.toString())
- Here we actually retrieve the account we created and then access totalGifs. When I run this via anchor test, I get:

🚀 Starting test...
📝 Your transaction signature 2KiCcXmdDyhMhJpnYpWXQy3FxuuqnNSANeaH1CBjvomuLZ8LfzDKHtDDB2LHfsfoVQZSyxoF1R39ao6VfTrD7bG7
👀 GIF Count 0
Yay! It's 0! This is pretty freaking epic. We now are actually calling a program and storing data in a permissionless manner on the Solana chain. NICE.

### Build a function to update GIF counter
Let's actually create a new function named add_gif that lets us actually increment the GIF counter. Check out some of my changes below.

use anchor_lang::prelude::*;
declare_id!("Fg6PaFpoGXkYsidMpWTK6W2BeZ7FEfcYkg476zPFsLnS");
#[program]
pub mod myepicproject {
  use super::*;
  pub fn start_stuff_off(ctx: Context<StartStuffOff>) -> Result <()> {
    let base_account = &mut ctx.accounts.base_account;
    base_account.total_gifs = 0;
    Ok(())
  }
	// Another function woo!
  pub fn add_gif(ctx: Context<AddGif>) -> Result <()> {
    // Get a reference to the account and increment total_gifs.
    let base_account = &mut ctx.accounts.base_account;
    base_account.total_gifs += 1;
    Ok(())
  }
}
#[derive(Accounts)]
pub struct StartStuffOff<'info> {
  #[account(init, payer = user, space = 9000)]
  pub base_account: Account<'info, BaseAccount>,
  #[account(mut)]
  pub user: Signer<'info>,
  pub system_program: Program <'info, System>,
}
// Specify what data you want in the AddGif Context.
// Getting a handle on the flow of things :)?
#[derive(Accounts)]
pub struct AddGif<'info> {
  #[account(mut)]
  pub base_account: Account<'info, BaseAccount>,
}
#[account]
pub struct BaseAccount {
    pub total_gifs: u64,
}
- Pretty simple! Near the bottom I added:
	
#[derive(Accounts)]
pub struct AddGif<'info> {
  #[account(mut)]
  pub base_account: Account<'info, BaseAccount>,
}
- I create a Context named AddGif that has access to a mutable reference to base_account. That's why I do #[account(mut)]. Basically it means I can actually change the total_gifs value stored on BaseAccount.

Otherwise, I may change data on it within my function but it wouldn't actually change on my account. Now, w/ a "mutable" reference if I mess w/ base_account in my function it'll change data on the account itself.

Last, I create a lil add_gif function!

pub fn add_gif(ctx: Context<AddGif>) -> Result <()> {
    // Get a reference to the account and increment total_gifs.
    let base_account = &mut ctx.accounts.base_account;
    base_account.total_gifs += 1;
    Ok(())
}
- All I do is grab the base_account which was passed in to the function via Context<AddGif>. Then, I increment the counter and that's it!!

Hope you can kinda see how the Context we set up near the bottom of the program actually becomes useful within the function. It's basically a nice way to say, "Hey, when someone calls add_gif be sure to attach the AddGif context to it as well so the user can access the base_account and whatever else is attached to AddGif.

### Update the test script...again!
Every time we update our program, we need to change up our script to test the changes! Let's update myepicproject.js to call add_gif.

const anchor = require('@project-serum/anchor');
const { SystemProgram } = anchor.web3;
const main = async() => {
  console.log("🚀 Starting test...")
  const provider = anchor.AnchorProvider.env();
  anchor.setProvider(provider);
  const program = anchor.workspace.Myepicproject;
  const baseAccount = anchor.web3.Keypair.generate();
  let tx = await program.rpc.startStuffOff({
    accounts: {
      baseAccount: baseAccount.publicKey,
      user: provider.wallet.publicKey,
      systemProgram: SystemProgram.programId,
    },
    signers: [baseAccount],
  });
  console.log("📝 Your transaction signature", tx);
  let account = await program.account.baseAccount.fetch(baseAccount.publicKey);
  console.log('👀 GIF Count', account.totalGifs.toString())
  // Call add_gif!
  await program.rpc.addGif({
    accounts: {
      baseAccount: baseAccount.publicKey,
    },
  });
  // Get the account again to see what changed.
  account = await program.account.baseAccount.fetch(baseAccount.publicKey);
  console.log('👀 GIF Count', account.totalGifs.toString())
}
const runMain = async () => {
  try {
    await main();
    process.exit(0);
  } catch (error) {
    console.error(error);
    process.exit(1);
  }
};
runMain();
- When I run this via anchor test I get:

🚀 Starting test...
📝 Your transaction signature 2Z9LZc5sFr8GHvwjZkrkqGJZ1hFNzZq2rTPV7jSEUjFoMZ16QQwPS2B7qqyNrmfFEpodHTBhvt5oSBi958mbwiR8
👀 GIF Count 0
👀 GIF Count 1
NICE. We are now actually storing and changing data on our Solana program. Epic.

Note: You'll notice when you run anchor test again it'll start the counter from 0 again. Why? Well — basically it's because whenever we run anchor test we generate a key pair for our account via anchor.web3.Keypair.generate(). This will actually create a new account every time. On our web app — we'll make sure to address this properly. But for testing purposes it's useful because we can start w/ a fresh account every time we test.



### Store structs on our program.

Epic. We're storing data on our Solana program. Not many people know how to do this stuff, so, you should definitely feel like a bit of a wizard. This ecosystem is really early and you're at the center of the magic right now.

So, a counter is cool. But, we want to store more complex data!

Let's now set it up where we can store an array of structs with more data we care about like: a link to the gif and the public address of the person who submitted it. Then, we'd be able to retrieve this data on our client!

##3 Set up Vec
Check out some of the updates below:

use anchor_lang::prelude::*;
declare_id!("Fg6PaFpoGXkYsidMpWTK6W2BeZ7FEfcYkg476zPFsLnS");
#[program]
pub mod myepicproject {
  use super::*;
  pub fn start_stuff_off(ctx: Context<StartStuffOff>) -> Result <()> {
    let base_account = &mut ctx.accounts.base_account;
    base_account.total_gifs = 0;
    Ok(())
  }
  // The function now accepts a gif_link param from the user. We also reference the user from the Context
  pub fn add_gif(ctx: Context<AddGif>, gif_link: String) -> Result <()> {
    let base_account = &mut ctx.accounts.base_account;
    let user = &mut ctx.accounts.user;
	// Build the struct.
    let item = ItemStruct {
      gif_link: gif_link.to_string(),
      user_address: *user.to_account_info().key,
    };	
	// Add it to the gif_list vector.
    base_account.gif_list.push(item);
    base_account.total_gifs += 1;
    Ok(())
  }
}
#[derive(Accounts)]
pub struct StartStuffOff<'info> {
  #[account(init, payer = user, space = 9000)]
  pub base_account: Account<'info, BaseAccount>,
  #[account(mut)]
  pub user: Signer<'info>,
  pub system_program: Program <'info, System>,
}
// Add the signer who calls the AddGif method to the struct so that we can save it
#[derive(Accounts)]
pub struct AddGif<'info> {
  #[account(mut)]
  pub base_account: Account<'info, BaseAccount>,
  #[account(mut)]
  pub user: Signer<'info>,
}
// Create a custom struct for us to work with.
#[derive(Debug, Clone, AnchorSerialize, AnchorDeserialize)]
pub struct ItemStruct {
    pub gif_link: String,
    pub user_address: Pubkey,
}
#[account]
pub struct BaseAccount {
    pub total_gifs: u64,
	// Attach a Vector of type ItemStruct to the account.
    pub gif_list: Vec<ItemStruct>,
}
- Starting from the bottom again, you'll see BaseAccount now has a new param named gif_list.  It's of type Vec which is basically short for Vector. You can read about them here. It's basically an array! In this case, it holds an array of ItemStructs.

Then I have this fancy piece of code to declare my ItemStruct.

#[derive(Debug, Clone, AnchorSerialize, AnchorDeserialize)]
pub struct ItemStruct {
    pub gif_link: String,
    pub user_address: Pubkey,
}
- It just holds a String w/ a gif_link and a PubKey w/ the user's user_address. Pretty straightforward. We also have this craziness:

#[derive(Debug, Clone, AnchorSerialize, AnchorDeserialize)]
- It's a little complex, but, basically this tells Anchor how to serialize/deserialize the struct. Remember, data is being stored in an "account" right? That account is basically a file and we serialize our data into binary format before storing it. Then, when we want to retrieve it we'll actually deserialize it.

This line takes care of that to make sure our data is properly serialized/deserialized since we're creating a custom struct here.

How did I figure this stuff out? Well  — I actually just dig through the docs myself and just read the source code! I also ask questions in the Anchor Discord! Remember, this stuff is new and it's up to you to discover answers when the docs don't provide them.

### Update the test script and boom!
As always, we need to return to our test script! Here are the updates:

const anchor = require('@project-serum/anchor');
const { SystemProgram } = anchor.web3;
const main = async() => {
  console.log("🚀 Starting test...")
  const provider = anchor.AnchorProvider.env();
  anchor.setProvider(provider);
  const program = anchor.workspace.Myepicproject;
  const baseAccount = anchor.web3.Keypair.generate();
  let tx = await program.rpc.startStuffOff({
    accounts: {
      baseAccount: baseAccount.publicKey,
      user: provider.wallet.publicKey,
      systemProgram: SystemProgram.programId,
    },
    signers: [baseAccount],
  });
  console.log("📝 Your transaction signature", tx);
  let account = await program.account.baseAccount.fetch(baseAccount.publicKey);
  console.log('👀 GIF Count', account.totalGifs.toString())
  // You'll need to now pass a GIF link to the function! You'll also need to pass in the user submitting the GIF!
  await program.rpc.addGif("insert_a_giphy_link_here", {
    accounts: {
      baseAccount: baseAccount.publicKey,
      user: provider.wallet.publicKey,
    },
  });
  // Call the account.
  account = await program.account.baseAccount.fetch(baseAccount.publicKey);
  console.log('👀 GIF Count', account.totalGifs.toString())
  // Access gif_list on the account!
  console.log('👀 GIF List', account.gifList)
}
const runMain = async () => {
  try {
    await main();
    process.exit(0);
  } catch (error) {
    console.error(error);
    process.exit(1);
  }
};
runMain();
- Note: don't forget to pass addGif a GIF link where it says insert_a_giphy_link_here else you'll get a confusing error like: baseAccount not provided.

Nothing new here really! One of the magic moments for me was when I saw the output of console.log('👀 GIF List', account.gifList). It's so cool to be able to just attach data to an account and access data via the account.

It's a really weird and new way to think about storing data, but it's pretty cool!!!

Here's what my output looked like upon doing 'anchor test'.

🚀 Starting test...
📝 Your transaction signature 3CuBdZx8ocXmzXRctvKkhttWHpP9knvAZnXQ9XyNcgr1xeqs6E3Hj9RVkEWSc2iEW15xXprKzip1hQw8o5kWVgsa
👀 GIF Count 0
👀 GIF Count 1
👀 GIF List [
  {
    gifLink: 'insert_a_giphy_link_here',
    userAddress: PublicKey {
      _bn: <BN: 69f90845012df1b26922a9e895b073309e647c36e9052f7c9ec29793b8be9e99>
    }
  }
]
We've gotten pretty far. We're now not only writing and running Solana programs, but, we've figured out how to store some complex data now as well!
	
	
	
## Deploy program to the devnet.

We pretty much have a basic API now — right :)?

We're able to POST data and we're able to GET data.

Let's hook up our web app with our program! What we'll need to do is deploy to the devnet. This is a network run by Solana that runs on fake SOL.

Technically, we could deploy our program locally w/ 'solana-test-validator' and build our web app using the local program — but here at buildspace we're pretty interested in getting to prod as quickly as possible :). Why mess around locally when we can deploy to the actual blockchain!?! Hehe.

Plus, I think it's easier to build the web app once we deploy our Solana program to devnet! Let's do this.

Note: Make sure 'solana-test-validator' is not running anywhere.

### Set up your environment for devnet
It's actually pretty tricky to deploy to devnet. Stay with me here and make sure not to miss any steps :).

First, switch to devnet:

'solana config set --url devnet'
- Once you do that, run:

'solana config get'
- And you'll see that you're now pointing to https://api.devnet.solana.com. This is how Anchor will know where to deploy to!

From here, we'll need to airdrop ourselves some SOL on the devnet. It's actually pretty easy, we just run twice:

'solana airdrop 2'
- Now, run:

'solana balance'
- And you should see 4 fake SOL in your wallet! This is actually retrieving your balance on devnet!

Note: sometimes you'll get an error that says something like "insufficient funds" — whenever that happens just airdrop yourself 2 SOL like above. Note: 2 is the max you can airdrop yourself at a time right now. So, you'll need to refresh your wallet occasionally.

### Changing up some variables
Now, we need to change some variables in Anchor.toml. This is where it gets a little tricky.

In Anchor.toml, change [programs.localnet] to [programs.devnet].

Then, change cluster = "localnet" to cluster = "devnet".

Now, run:

'anchor build'
- This will create a new build for us with a program id. We can access it by running:

'solana address -k target/deploy/myepicproject-keypair.json'
- This will output your program id, copy it. We'll talk about it more in a second.

Now, go to lib.rs. You'll see this id at the top.

declare_id!("Fg6PaFpoGXkYsidMpWTK6W2BeZ7FEfcYkg476zPFsLnS");
- So...what is this? It confused me a lot when I was first researching lol.

Basically, it's an id initially generated by anchor init that specifies our program's id.

That's important because the id of the program specifies how to load and execute the program and contains info on how the Solana runtime should execute the program.

The program id also helps the Solana runtime see all the accounts created by the program itself — so, remember how our program creates "accounts" that hold data related to the program? Well, with this ID Solana can quickly see all the accounts generated by the program and easily reference them.

So, we need to change this program id in declare_id! to the one output by 'solana address -k target/deploy/myepicproject-keypair.json'. Why? Well, the one 'anchor init' gave us was just a placeholder. Now we'll have a program id we can actually deploy with!

Note: everyone's program id will be different! It's generated by Anchor.

Now, go to Anchor.toml and under [programs.devnet] you'll see something like myepicproject = "Fg6PaFpoGXkYsidMpWTK6W2BeZ7FEfcYkg476zPFsLnS". Go ahead and change this id to the same id output when you run 'solana address -k target/deploy/myepicproject-keypair.json'.

Great, so you changed your program id in two spots!

Finally, once you do all this we need to run the build command again:

'anchor build'
- Why? Because we want to actually build the project w/ our new program id! Anchor generates certain files upon a build under the target directory and we want to make sure those generated files have the latest and greatest program id.

That was a lot of steps. It's easy to mess up and get a random, confusing error. To summarize:

'solana config set --url devnet'

// Make sure you're on devnet.
'solana config get'

'anchor build'

// Get the new program id.
'solana address -k target/deploy/myepicproject-keypair.json'

// Update Anchor.toml and lib.rs w/ new program id.
// Make sure Anchor.toml is on devnet.

// Build again.
'anchor build'
	
### Deploy to devnet!
And finally, you're good to deploy :)! Go ahead and run:

'anchor deploy'
You should see the word "Deploy success" :).

Once you do, you head to the Solana Explorer to see if it all worked! Note: make sure you go to the top right, click "Mainnet" and then click "Devnet" since we deployed to Devnet.

On the explorer, paste in your program id (the same one we had from 'solana address -k target/deploy/myepicproject-keypair.json') and search for it.
	
You'll see your deployed program!! Scroll down and see the transaction history and you'll see the deployment right there.

YO — YOU JUST DEPLOYED TO THE ACTUAL SOLANA BLOCKCHAIN. NICE.

This of course is not "Mainnet", but the "Devnet" is run by actual miners and is legit.
	
	
	
## Running a test on devnet
I actually like to run an anchor test at this point before I start integrating my web app. This could potentially save us from some random, annoying errors.

Because our config is set to devnet, Anchor will actually run our tests directly on the devnet which is exactly what we want. That way we can make sure our actual functions are working properly on devnet!

Go ahead run it using:

'anchor test'
- As long as nothing crashed and it's showing you something like:

Deploy success
🚀 Starting test...
📝 Your transaction signature 41aW8pAtFLyxgg1S54EATUSKSXB9LKe1qSGgLvuy3Fh58vWgiHuXK8jsrtRy5Spm32xCytXoNyJTMKVpa4ZHcnEB
👀 GIF Count 0
👀 GIF Count 1
👀 GIF List [
  {
    gifLink: 'insert_a_giphy_link_here',
    userAddress: PublicKey {
      _bn: <BN: 368327095334a46e8bf98ccfd43f4662111b633d3989f6f9df869306bcc64458>
    }
  }
Then you're good to go.

What's actually interesting here is if you go to the Solana Explorer and paste in your program id (just like you did before) you'll see some new transactions there. These were caused by the test you just ran! Feel free to check them out.

I should mention something super important here. When you ran anchor test just now, it'll actually re-deploy the program and then run all the functions on the script.

You may be asking yourself, "Why did it re-deploy? Why isn't it just talking to the program already deployed? Also, if we re-deployed wouldn't it have been deployed to a completely different program id?".

So — Solana programs are upgradeable. That means when we re-deploy we're updating the same program id to point to the latest version of the program we deployed. And, what's cool here is the accounts that the programs talk to will stick around — remember, these accounts keep data related to the program.

That means we can upgrade programs while keeping the data piece separate. Pretty cool, right :)?

Note: this is very very different from Ethereum where you can never change a smart contract once it's deployed!

### Hooking up our IDL file to the web app
So, we now have a deployed Solana program. Let's connect it up to our web app :).

The first thing we need is the idl file that was magically output by anchor build earlier without you knowing. You should see it in target/idl/myepicproject.json.

The idl file is actually just a JSON file that has some info about our Solana program like the names of our functions and the parameters they accept. This helps our web app actually know how to interact w/ our deployed program.

You'll also see near the bottom it has our program id! This is how our web app will know what program to actually connect to. There are millions of programs deployed on Solana and this address is how our web app can get quick access to our program specifically.

Note: if you don't see the idl file or you don't see an "address" parameter near the bottom, then something has gone wrong! Start again from the "Deploy program to the devnet" section of the project.

Go ahead and copy all the content in target/idl/myepicproject.json.

Head over to your web app.

In the src directory of your react app create an empty file named idl.json. It should be in the same directory as App.js. So for me, I have the file at app/src/idl.json. Once you create the file, paste the content of target/idl/myepicproject.json into your newly created app/src/idl.json.

Finally, in App.js, go ahead and drop this in as an import:

import idl from './idl.json';
Nice!!
