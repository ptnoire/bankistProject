# bankistProject
PROJECT: Bankist App

Writing this for the README for my Project, the following is listed in the order that I learned and coded the JavaScript for this project. 
The HTML / CSS is not my own, it was created by Jonas Schmedtmann, the person whose course I learned this from.

Below is all the parts together that I’ve done in order and my thoughts on them as I was coding it, this is going to be a very primitive 
read as they are just free flowing thoughts. My plan in the future is to redo this code and readme once I feel completely comfortable with 
all this subject matter, except I will design the HTML and CSS, plus work only from the flow chart.

My final thoughts on this project is, I feel 100% confident in coding this, I had a lot of fun and I am very proud to of come this far even 
if this project is just a feature showcase. I have thoughts on how I can implement it into the real world and I am excited to move forward.

This was originally presented as a google doc, I know that documentation should be written in code style, in the future I will keep this in mind.

It is presented as a single code block, then a dissect of each section that is important to give my thoughts on it.

Displaying the transactions (movements) on the DOM

// Data (We use objects in this case to simulate web API intake)
const account1 = {
  owner: 'Jonas Schmedtmann',
  movements: [200, 450, -400, 3000, -650, -130, 70, 1300],
  interestRate: 1.2, // %
  pin: 1111,
};

// Code
const displayMovements = function(movements) {
  containerMovements.innerHTML = '';
 
  movements.forEach(function(mov, i){
    const type = mov > 0 ? 'deposit' : 'withdrawal'
 
    const html = `
    <div class="movements__row">
      <div class="movements__type movements__type--${type}">${i + 1} ${type}</div>
      <div class="movements__value">${mov}</div>
    </div>`
 
    containerMovements.insertAdjacentHTML('afterbegin', html);
  })
}
displayMovements(account1.movements);

// Line by Line

Grabbing the HTML we’re going to be working with.
const containerMovements = document.querySelector('.movements');

Instead of working with raw global data we’re going to get a function to do it for us so that our code is nice and clean. (in example if this is all bad, we can just remove the function as opposed to searching through the code to find out what is going wrong)
const displayMovements = function(movements) {
  containerMovements.innerHTML = '';

container.Movements.innerHTML = ‘’;
What we’re doing here with this line is removing the old contents before adding more. Essentially innerHTML works like innerText except it grabs all of the html versus all of the text. 

We are making sure here that we know what type of movement is happening within the bank by checking if the number passed is positive or negative. We will then store this as a variable to use later to format how the data comes out.
  movements.forEach(function(mov, i){
    const type = mov > 0 ? 'deposit' : 'withdrawal'


Now here is something new:
	We are defining a variable as a html string to be inserted with some dynamic templates to match our code.
    const html = `
    <div class="movements__row">
      <div class="movements__type movements__type--${type}">${i + 1} ${type}</div>
      <div class="movements__value">${mov}</div>
    </div>`

Some notes: our class CSS is designed around being a deposit (green css) or withdrawal (red css) so in the variable ‘type’ which we defined just above, we make sure this matches with one of these classes so that we can dynamically change which css is used per line.

We take the index (variable i) to show what transaction number it is, this is a practice we are used to when we were working within the console.

Then we add the value via the variable mov.

All of this is similar to how we would do it in the console except for this time we are using HTML and CSS to manipulate the DOM to display this information.

Now this is a call function we haven’t used before and a term that is referenced as well.
	.insertAdjacentHTML will put this html in a certain spot (defined by the first argument within the function, there is documentation on this particular definitions I will make a note of) and then the 2nd argument is what variable we are passing, in this case we defined html to be the html we wanted to pass through.

insertAdjacentHTML notes: 

The insertAdjacentHTML() method does not reparse the element it is being used on, and thus it does not corrupt the existing elements inside that element. This avoids the extra step of serialization, making it much faster than direct innerHTML manipulation.

<!-- beforebegin -->
<p>
  <!-- afterbegin -->
  foo
  <!-- beforeend -->
</p>
<!-- afterend -->

    containerMovements.insertAdjacentHTML('afterbegin', html);
  })
}

Then of course we execute the function.
displayMovements(account1.movements);


Making a username based on the initials of the owner’s name

// Data
const account1 = {
  owner: 'Jonas Schmedtmann',
  movements: [200, 450, -400, 3000, -650, -130, 70, 1300],
  interestRate: 1.2, // %
  pin: 1111,
};
 
const account2 = {
  owner: 'Jessica Davis',
  movements: [5000, 3400, -150, -790, -3210, -1000, 8500, -30],
  interestRate: 1.5,
  pin: 2222,
};
 
const account3 = {
  owner: 'Steven Thomas Williams',
  movements: [200, -200, 340, -300, -20, 50, 400, -460],
  interestRate: 0.7,
  pin: 3333,
};
 
const account4 = {
  owner: 'Sarah Smith',
  movements: [430, 1000, 700, 50, 90],
  interestRate: 1,
  pin: 4444,
};
const accounts = [account1, account2, account3, account4];

// Code
const makeUserName = function(accs){
  accs.forEach(function(acc){
    acc.userName = acc.owner
    .toLowerCase()
    .split(' ')
    .map(word => word[0]).join('');
  })
}
makeUserName(accounts);

// Line by Line

We are making a new function to create a username based off the user’s initials.
const makeUserName = function(accs){

The above function will take in the argument accs (which we intended to put the array ‘accounts’ which pulls from the objects listed a bove)
  accs.forEach(function(acc){

Here we are creating a new key value on the original object called ‘userName’
    acc.userName = acc.owner

We use a few string calls to make the data look nice, lower casing it all, splitting it by spaces (basically taking each name, some names have 3 words)
    .toLowerCase()
    .split(' ')

Now we are using the .map() method to create a new array, let me break this down further below
    .map(word => word[0]).join('');
  })
}

Inside the map call () we have an arrow function which will take the first argument named word, this argument is going to be tied to the elements within the array we are calling it on.  In this case let’s trace back to what we are doing since there are a few calls.

acc.owner.toLowerCase().split(‘ ‘).map()
				^-- This is the array we created by splitting and so we are creating a new
				array from this.

So… .map() is going to create a new array based off the function that is inside the call, that function which in the code we have an arrow function looks like this.

.map(word => word[0].join(‘’);
             ^-- each element from .split(‘ ‘) which would be each word.
	Then we return (=>) the first index of each word and then join it together with an empty string.

Then finally we call the function with the full array of all the accounts.
makeUserName(accounts);


Displaying a certain account’s balance based off the total of the movements

// Data
const account1 = {
  owner: 'Jonas Schmedtmann',
  movements: [200, 450, -400, 3000, -650, -130, 70, 1300],
  interestRate: 1.2, // %
  pin: 1111,
};
const labelBalance = document.querySelector('.balance__value');
// Code
const calcDisplayBalance = function(movements){
  const balance = movements.reduce((acc, cur) => acc + cur, 0);
  labelBalance.textContent = `${balance} EUR`
}
calcDisplayBalance(account1.movements);
// Line by Line

Making a function to calculate and display the balance, a function that takes in something and names it a variable movements
const calcDisplayBalance = function(movements){

Here we are using the reduce method call function
  const balance = movements.reduce((acc, cur) => acc + cur, 0);

Just for sake of clarity, it works like so:
Whatever array goes into the function will then be called on by the reduce method.
The reduce method will take in the accumulator (a rolling number just for this call function)
The array elements ‘cur’
Then we use an arrow function to take both of these 2 arguments and return them together
Then the final , 0 is just where the accumulator number should start.

The variable labelBalance is tied to the document, see data above and we’re just changing it’s text content dynamically to match the current balance + EUR in this instance, may change the currency label later.
  labelBalance.textContent = `${balance} EUR`
}

Then you already know we gotta call that shit.
calcDisplayBalance(account1.movements);


Calculating and Displaying a Summary (in, out and interest)

// Code
const calcDisplaySummary = function(movements) {
  const incomes = movements
  .filter(mov => mov > 0)
  .reduce((acc, mov)=> acc + mov, 0);
  labelSumIn.textContent = `${incomes} €`;
  const outcomes = movements
  .filter(mov => mov < 0)
  .reduce((acc, mov)=> acc - mov, 0);
  labelSumOut.textContent = `${outcomes} €`;
  const interest = movements
  .filter(mov => mov > 0)
  .map(deposit => deposit * 1.2 / 100)
  .filter(dep => dep >= 1)
  .reduce((acc, mov)=> acc + mov, 0);
  labelSumInterest.textContent = `${interest} €`
}
calcDisplaySummary(account1.movements);
// Line by Line

const calcDisplaySummary = function(movements) {

First thing we are doing is filtering positive movements, which would obviously be deposits or in this case income, then reducing all of the array into a single number, then displaying it on the DOM.
  const incomes = movements
  .filter(mov => mov > 0)
  .reduce((acc, mov)=> acc + mov, 0);
  labelSumIn.textContent = `${incomes} €`;

Next we’re doing the same thing but with the negative movements, which are withdrawals.
  const outcomes = movements
  .filter(mov => mov < 0)
  .reduce((acc, mov)=> acc - mov, 0);
  labelSumOut.textContent = `${outcomes} €`;

Then we calculate interest (in our example the bank pays 1.2% interest, we calculate this by 1.2 divided by 100)
  const interest = movements
  .filter(mov => mov > 0)
  .map(deposit => deposit * 1.2 / 100)

Then we added a rule in the bank that it only gives interest on deposits over a dollar (1) then we finally display this on the DOM.
  .filter(dep => dep >= 1)
  .reduce((acc, mov)=> acc + mov, 0);
  labelSumInterest.textContent = `${interest} €`
}
calcDisplaySummary(account1.movements);


LOGIN FUNCTIONALITY AND DYNAMIC UPDATING

This is a big one,  so let’s breaker down.

// Code
// LOGIN
btnLogin.addEventListener('click', function(e){
  // Prevent for from submitting (A button's default option is to refresh the page)
  e.preventDefault();
  // Getting correct login information (initials: username, and PIN)
  const currentAccount = accounts.find(acc => acc.userName === inputLoginUsername.value);
  if (Number(inputLoginPin.value) === currentAccount?.pin) {
    console.log('Login Successful')
    // Display UI, change opacity in .app to 100 from 0
    containerApp.style.opacity = 100;
    labelWelcome.textContent = `Welcome ${currentAccount.owner.split(' ')[0]}!`;
    // Display and Calculate Balances for account.
    displayMovements(currentAccount.movements);
    calcDisplayBalance(currentAccount.movements);
    calcDisplaySummary(currentAccount);
    // Clear username / pin fields.
    inputLoginUsername.value = inputLoginPin.value = '';
    inputLoginPin.blur();
  }
})
// Line by Line

Just adding a event listener to the button.
// LOGIN
btnLogin.addEventListener('click', function(e){

So I found out that the default functionality for a button in HTML is to refresh the page when it is clicked, so I learned a nifty call method called preventDefault which you will see used below. Probably would of never figured this out if it wasn’t told to me so keep this in mind in the future when coding websites or such. A note: the console would flash with my log message then disappear, it was the sign that something was going on.
  // Prevent for from submitting (A button's default option is to refresh the page)
  e.preventDefault();

Here we are creating a variable that will store the account that is attached to the username we put in, let me break it down below just to make sure I and future me understand this:
  // Getting correct login information (initials: username, and PIN)
  const currentAccount = accounts.find(acc => acc.userName === inputLoginUsername.value);
  if (Number(inputLoginPin.value) === currentAccount?.pin) {
    console.log('Login Successful')

accounts.find(acc => acc.userName === inputLoginUsername.value)
	What is going on here is 2 dynamic variables working in harmony.

The find method will take the input we put into the user login field on the html (which is tied to the variable inputLoginUsername) and match it to an object that is within the variable accounts (which if you don’t remember is an array that holds all of our accounts.)
To clarify just in case: ‘acc’ is the argument we input when we call the function.
We are using the argument ‘accounts’ which is an array with all of our accounts in it, each one of those accounts is an object.
.find() will loop over each element in the array til it matches the inputted data with what it finds in that objects ‘userName’ key (Which should match the initials inputted in the html field for username)
Now the account that it found, is saved as the currentAccount variable which we can recall later for the rest of this code.

Then we convert the inputLoginPin.value to a number (because by default it gets entered as a string) and then make sure it matches to the account we found in our .find() call method to basically see if they line up. (Important!! Note the optional chaining we use to make sure that this a account actually exists, it won’t continue the code or will return undefined if it doesn’t exist vs an error)

In the end we end up with a variable that stores the current account object that is being logged in, now we have to use that specific object’s keys to dynamically update the rest of the page’s information which we will come back to in a second.


containerApp (is tied to the class .app, which is tied to the CSS of the html page) we are using .style in conjunction with .opacity to change it from 0 to 100 basically making it visible from invisible.

Then we are just changing the welcome text to the user’s first name (using the split method)
    // Display UI, change opacity in .app to 100 from 0
    containerApp.style.opacity = 100;
    labelWelcome.textContent = `Welcome ${currentAccount.owner.split(' ')[0]}!`;

Calling all 3 of our previous functions, later on I will probably make a start up function vs have 3 of these at the same time.
    // Display and Calculate Balances for account.
    displayMovements(currentAccount.movements);
    calcDisplayBalance(currentAccount.movements);
    calcDisplaySummary(currentAccount);

We are clearing the values in the html fields so they don’t stick around after we have logged in, then blur just changes the focus so that the blinking cursor doesn’t stay in the pin field.
    // Clear username / pin fields.
    inputLoginUsername.value = inputLoginPin.value = '';
    inputLoginPin.blur();
  }
})


Transfer Functionality
// Code
// Transfer Function
btnTransfer.addEventListener('click', function(e){
  e.preventDefault();
    // Get the account money is transfering from:
  const xferFromAcc = accounts.find(acc => acc.owner.split(' ')[0] === labelWelcome.textContent.split(' ')[2])
    // Get the account money is transfering to
  const xferToAcc = accounts.find(acc => acc.userName === inputTransferTo.value)
  // get the amount of money being transfered
  const xferAmmount = Number(inputTransferAmount.value);
 
  // Add a movement to both accounts then use previous functions to do the work.
  if (xferAmmount > 0 &&
    xferToAcc &&
    xferFromAcc.balance >= xferAmmount &&
    xferToAcc?.userName !== xferFromAcc.userName){
      xferToAcc.movements.push(xferAmmount);
      xferFromAcc.movements.push(-Math.abs(xferAmmount)); // -Math.abs gives a negative value
      inputTransferAmount.value = inputTransferTo.value = '';
      updateUI(xferFromAcc)
  }
  inputTransferAmount.value = inputTransferTo.value = '';
})

// Line by Line
When clicking on the transfer button, prevents default (refreshing page)
// Transfer Function
btnTransfer.addEventListener('click', function(e){
  e.preventDefault();


This is getting the current account that is logged in, there is a better way to do this but why I did it this way was just to practice getting items from the DOM and using them in the back end, this particularly takes the first name from the welcome message at the top of the DOM that updates when you login.

The real way to do this is just define current account in the global scope and update it through the login function, which I will go back and write just because it’s going to be used more than once but just wanted to show off this code for shits and giggles.
    // Get the account money is transfering from:
  const xferFromAcc = accounts.find(acc => acc.owner.split(' ')[0] === labelWelcome.textContent.split(' ')[2])

Same way we find the current account by comparing the values with the .find() call.
    // Get the account money is transfering to
  const xferToAcc = accounts.find(acc => acc.userName === inputTransferTo.value)

Converting the amount entered into a number and then defining it in a variable to be used later.
  // get the amount of money being transfered
  const xferAmmount = Number(inputTransferAmount.value);

We first check to see if:
The amount is above 0, so no negative amounts can be pushed through.
The account we want to transfer to actually exists
The current user has enough money to make the transfer
We are not transferring money to ourselves.

If this is all true, we push the value through the movements array in the account object then push a negative version of that movement through the current account, so that our other functions work perfectly well.

We then redefine a few things which I’ll add at the bottom to update the rest of the code (updateUI function and adding .balance to the keys of the accounts)
 
  // Add a movement to both accounts then use previous functions to do the work.
  if (xferAmmount > 0 &&
    xferToAcc &&
    xferFromAcc.balance >= xferAmmount &&
    xferToAcc?.userName !== xferFromAcc.userName){
      xferToAcc.movements.push(xferAmmount);
      xferFromAcc.movements.push(-Math.abs(xferAmmount)); // -Math.abs gives a negative value
      inputTransferAmount.value = inputTransferTo.value = '';
      updateUI(xferFromAcc)
  }

Clear the text fields (good indication that something happened)
  inputTransferAmount.value = inputTransferTo.value = '';
})



In good practice we redefined all of these functions so that we can use one call. I updated the rest of the code to reflect this.
const updateUI = function(acc) {
  displayMovements(acc.movements)
  calcDisplayBalance(acc)
  calcDisplaySummary(acc)
}

Instead of just using the const inside the function, I added the balance as a key to the object which can be found later.
const calcDisplayBalance = function(account){
  const balance = account.movements.reduce((acc, cur) => acc + cur, 0);
  labelBalance.textContent = `${balance} €`
  // Adding a balance to the account each time this function is called.
  account.balance = balance;
}


Close Account Function (findIndex of)

// Close Account
btnClose.addEventListener('click', function(e) {
  e.preventDefault();
  if (currentAccount.userName == inputCloseUsername.value &&
     currentAccount.pin == inputClosePin.value) {
      const index = accounts.findIndex(acc => acc.userName === currentAccount.userName)
      console.log(index)
      accounts.splice(index, 1);
      containerApp.style.opacity = 0;
      labelWelcome.textContent = `Log in to get started`;
      inputClosePin.value = inputCloseUsername.value = '';
  }
})

I’m going to be brief with this one, why you would add this function is kinda weird but let’s just go over the one thing that is important

The findIndex of (at const index) is just finding the account in the array (accounts) so that we can close it. We ‘close’ the account by splicing it from the array and then setting the page back to how we found it, making the interface opacity  0 and changing the text  back at the top, simple stuff won’t go too detailed into it.

Loan Functionality

// Loan Button
btnLoan.addEventListener('click', function(e) {
  e.preventDefault();
  const amount = Number(inputLoanAmount.value);
  console.log(amount);
  if (amount > 0 && currentAccount.movements.some(mov => mov > amount * .1)) {
    console.log('worked.')
    currentAccount.movements.push(amount);
    updateUI(currentAccount);
  }
  inputLoanAmount.value = '';
})

This is straight forward but it uses the .some function which is just checking if the person has made a deposit that is at least 0.1% of the loan they are requesting before getting the loan.

.some(mov => mov > amount * .1) if true -> statement executes


The sort function

Going to be pulling a few different lines of code and changing it to add features and show them off.
const displayMovements = function(movements, sort = false) {

We are changing how this function works to check the ‘sort’ variable which is of course created in the function.
  const movs = sort ? movements.slice().sort((a, b) => a - b) : movements;

Here we are implementing a basic sort callback method but using the slice callback to make sure we are not using the original array (because sort will mutate the original array)

If sort is true then we will be using the slice method of this array that is sorted via the condition we set which is that if a is greater than b than it returns a positive value. Since the sort method technically will read everything as strings the reason we are doing this is because we want the sort method to conditionally show numbers in a certain order so it keeps looping over the array putting things positive first and negative last until it is done.

movs.forEach(function(mov, i){
Changed the above from ‘movements’ which is the first argument to movs which is the new variable we made to check if sort is true or not.

let sortState = false;
btnSort.addEventListener('click', function(e){
  e.preventDefault();
  displayMovements(currentAccount.movements, !sortState);
  sortState = !sortState;
})

Then we added a sort button event listener, this will call the function ‘displayMovements’
The first argument is straight forward, we call the function like we do normally but the second argument is going to be ‘not sortState’ which because this is a boolean it will be the opposite of whatever the actual boolean of sortState is, so it will make the argument true if it is false at the time.

Then the line after that is basically the toggle, so we set sortState to !sortState (not sortState) meaning that whatever sortState isn’t, it will now become. (A simple toggle feature)


Making sure all the numbers are with 2 decimal places
Simple thing: Just added .toFixed(2) to any place we had numbers pump out into the DOM, some examples:
const calcDisplaySummary = function(mov) {
  const incomes = mov.movements
  .filter(mov => mov > 0)
  .reduce((acc, mov)=> acc + mov, 0);
  labelSumIn.textContent = `${incomes.toFixed(2)} €`;

const displayMovements = function(movements, sort = false) {
  containerMovements.innerHTML = '';
 
  const movs = sort ? movements.slice().sort((a, b) => a - b) : movements;
 
  movs.forEach(function(mov, i){
    const type = mov > 0 ? 'deposit' : 'withdrawal'
    const html = `
    <div class="movements__row">
      <div class="movements__type movements__type--${type}">${i + 1} ${type}</div>
      <div class="movements__value">${mov.toFixed(2)} €</div>
    </div>`
    containerMovements.insertAdjacentHTML('afterbegin', html);
  })
}


Adding timestamps / dates to everything
    const now = new Date();
      const day = `${now.getDate()}`.padStart(2, 0);
      const month = `${now.getMonth() + 1}`.padStart(2, 0);
      const year = now.getFullYear();
      const hour = `${now.getHours()}`.padStart(2, 0);
      const minute = `${now.getMinutes()}`.padStart(2, 0);
      labelDate.textContent = `${day}/${month}/${year}, ${hour}:${minute}`;

Converting it to a string to add the ‘padstart(2, 0)’ which is just a way to make sure that if the date is the 2nd, it will show up as 02 same with anything that may have that come up.

We add +1 to month because the month is 0 based, so it will always be 1 behind.

  movs.forEach(function(mov, i){
    const type = mov > 0 ? 'deposit' : 'withdrawal'
      const date = new Date(acc.movementsDates[i]);
      const dateYear = `${date.getFullYear()}`;
      const dateMonth = `${date.getMonth() + 1}`.padStart(2, 0);
      const dateDate = date.getDate();
      const dateHour = `${date.getHours()}`.padStart(2, 0);
      const dateMinute = `${date.getMinutes()}`.padStart(2, 0);
      const displayDate = `${dateYear}/${dateMonth}/${dateDate} ${dateHour}:${dateMinute}`
    const html = `
    <div class="movements__row">
      <div class="movements__type movements__type--${type}">${i + 1} ${type}</div>
      <div class="movements__date">${displayDate}</div>
      <div class="movements__value">${Number(mov).toFixed(2)} €</div>
    </div>`
    containerMovements.insertAdjacentHTML('afterbegin', html);

We add this setting to update the movements, so whenever this function is called it will also update the date on it. Note: at this point in time I was working with a different set of data, one that included a key called ‘movementDates’ which included a ISO string timestamp:

const account2 = {
  owner: 'Jessica Davis',
  movements: [5000, 3400, -150, -790, -3210, -1000, 8500, -30],
  interestRate: 1.5,
  pin: 2222,
 
  movementsDates: [
    '2019-11-01T13:15:33.035Z',
    '2019-11-30T09:48:16.867Z',
    '2019-12-25T06:04:23.907Z',
    '2020-01-25T14:18:46.235Z',
    '2020-02-05T16:33:06.386Z',
    '2020-04-10T14:43:26.374Z',
    '2020-06-25T18:49:59.371Z',
    '2020-07-26T12:01:20.894Z',
  ],
  currency: 'USD',
  locale: 'en-US',
};

      currentAccount.movementsDates.push(new Date().toISOString());
      xferToAcc.movementsDates.push(new Date().toISOString());

In the transfer function and the loan we added this .toISOstring and logged it into the objects so that it could be read by our code in the other functions.


Implementing a ‘today, yesterday or x days ago’ feature

We moved the formatting of the dates from above to it’s own function so now it can be called like so:
      const date = new Date(acc.movementsDates[i]);
 Into v
      const displayDate = formatMovementDate(date);



const formatMovementDate = function(date) {
  const calcDaysPassed = (date1, date2) => Math.abs(date2 - date1) / (1000 * 60 * 60 * 24);
We wrote a arrow function here to take in 2 dates and subtract them from each other to show how many days have passed.
The math behind this is simply converting date1 and date2 into timestamp numbers (which is a long string eventually just calculating the milliseconds) then we convert the large numbers down into a more readable version which is why we use the math (1000 (milliseconds) * 60 (seconds) * 60 (minutes) * 24 (hours) ) this will return just the days, or a number that can represent days.

  const daysPassed = Math.round(calcDaysPassed(new Date(), date));
Storing the result as a variable to be called simply later on:
  if (daysPassed === 0) return 'Today';
  if (daysPassed === 1) return 'Yesterday';
  if (daysPassed <= 7) return `${daysPassed} days ago`;
Choose the return value based on the variable that is returned, or if the days passed reaches a certain threshold then we just enter the string, if it is over the 0 or 1, then we take the variable and use it in a dynamic template to make the string.

  else {
  const dateYear = `${date.getFullYear()}`;
  const dateMonth = `${date.getMonth() + 1}`.padStart(2, 0);
  const dateDate = date.getDate();
  const dateHour = `${date.getHours()}`.padStart(2, 0);
  const dateMinute = `${date.getMinutes()}`.padStart(2, 0);
  return `${dateYear}/${dateMonth}/${dateDate} ${dateHour}:${dateMinute}`
}}

Otherwise, we just format a date as we did before.




Internationalizing date formats
const formatMovementDate = function(date, locale) {
  const calcDaysPassed = (date1, date2) => Math.abs(date2 - date1) / (1000 * 60 * 60 * 24);
  const daysPassed = Math.round(calcDaysPassed(new Date(), date));
  if (daysPassed === 0) return 'Today';
  if (daysPassed === 1) return 'Yesterday';
  if (daysPassed <= 7) return `${daysPassed} days ago`;
  else {

Removed the entire 6 variable date gathering and used the built in function to format the date based on the user’s country’s format. In our app’s current state it has an object that will define the ISO language code to change this.
  return new Intl.DateTimeFormat(locale).format(date)
}}


Internationalizing Currency Formats
We replace anywhere where it would display an amount to this function code here:

Important! Note that our account date has a locale and currency defined to put into the ‘style’ and ‘currency’ options, these need to be changed manually otherwise they don’t just automatically work.

const formatCur = function(value, locale, currency) {
  return Intl.NumberFormat(locale, {
    style: 'currency',
    currency: currency,
  }).format(value);
}


Security Logout (after 10 seconds for testing)
const startLogOutTimer = function() {
  let time = 10;
10 is 10 seconds, short so we can see it implemented, you can set it to 5 minutes with 5000 later

  const timer = setInterval(function(){
    const min = String(Math.trunc(time / 60)).padStart(2, 0);
    const sec = String(Math.trunc(time % 60)).padStart(2, 0);
This makes sure that our clock will display the minutes and seconds correctly.

-> We convert to a string to use padStart callback feature which makes sure that all single digits have a 0 in front of them for formatting.
-> We trunc to make sure the numbers are just whole numbers and no floats.
-> The time divided by 60 is out of 60 minutes in an hour.
-> the remainder out of 60 would be the seconds.
    labelTimer.textContent = `${min}:${sec}`;


We also had to globally define ‘timer’ as a variable so that we can check if it is truthy before we start a new timer.

We needed to change this code to basically do the match every second and update the timer everytime someone logs in:

So we cut the ‘timer’ section from before and just made it it’s own function, then called it just before we run the clock, then we show the clock right after with the returned value from ‘tick’
const startLogOutTimer = function() {
  const tick = function(){
    const min = String(Math.trunc(time / 60)).padStart(2, 0);
    const sec = String(Math.trunc(time % 60)).padStart(2, 0);
    labelTimer.textContent = `${min}:${sec}`;
    if (time === 0){
      labelWelcome.textContent = `Log in to get started`;
      containerApp.style.opacity = 0;
      clearInterval(timer)
    }
    time--;
  }
  let time = 120;
  tick();
  const timer = setInterval(tick, 1000)
  return timer;
}

If the time reaches 0, just clear the page with opacity 0 and set the welcome message back to whatever it starts as, clear the timer so it stops. Now if it isn’t zero, each interval update (which is set to 1000, which is 1 second) it will decrease the ‘time’ variable by 1.


Added to the ‘login’ event handler:
    inputLoginUsername.value = inputLoginPin.value = '';
    inputLoginPin.blur();
    if (timer) clearInterval(timer)
    timer = startLogOutTimer();
This makes sure there is no timer running on login / logout change accounts etc.
