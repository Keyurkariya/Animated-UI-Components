<script>
  // Javascript here
  let strength = 0;
  let validations = [];
  let showPassword = false;

  function checkPassowrd(e) {
    const passWord = e.target.value;

    validations = [
      passWord.length > 5,
      passWord.search(/[A-Z]/) >= 0,
      passWord.search(/[0-9]/) >= 0,
      passWord.search(/[!@#$%^&*]/) >= 0,
    ];
    strength = validations.reduce((a, b) => a + b);
  }
</script>

<style>
  /* CSS here */
  form {
    --text-color: white;
    max-width: 500px;
  }
  .field {
    width: 100%;
    position: relative;
    /* Defining the border for all field class using text color variable */
    border-bottom: 2px dashed var(--text-color);
    /*
       top-margin -> 4rem("Root Font Size") 
       left & right -> auto
       bottom -> 1rem
       margin are defined for seamless animation
    */
    margin: 4rem auto 1rem;
  }
  .label {
    color: var(--text-color);
    font-size: 1.2rem;
  }
  .input {
    outline: none;
    border: none;
    overflow: hidden;
    margin: 0;
    width: 100%;
    padding: 0.25rem 0;
    background: none;
    color: white;
    font-size: 1.2rem;
    font-weight: bold;
  }
  /* 🔮 Animation Magic Starts here 🔮 */
  /* Overiding the valid and invalid validation of html input field */
  .input:valid {
    color: yellowgreen;
  }
  .input:invalid {
    color: orangered;
  }
  /* Border Animation below the input field */
  .field::after {
    content: "";
    position: relative;
    display: block;
    height: 4px;
    width: 100%;
    background: blue;
    /* scaleX() as we only want to transform width */
    transform: scaleX(0);
    /* 
    border expands from 
    left to right 
    for other way around use 100% 
    */
    transform-origin: 0%;
    transition: transform 500ms ease;
    top: 2px;
  }
  /* 
  Using CSS Pseudo selector :focus-within 
  when the field is focused 
  */
  .field:focus-within {
    border-color: transparent;
  }
  .field:focus-within::after {
    transform: scaleX(1);
  }

  /* Lable Animation here */

  /* 
  Remember 🤔 till here we have the label below 
  the input field 
  */
  .label {
    z-index: -2;
    position: absolute;
    transform: translateY(-2rem);
    transform-origin: 0%;
    transition: transform 400ms;
  }
  /* 
  Here this means once transformed to top 
  if the placeholder is not being showned 
  i.e. user has done some input don't transform back to original
  */

  .field:focus-within .label,
  .input:not(:placeholder-shown) + .label {
    transform: scale(0.8) translateY(-5rem);
    font-weight: bolder;
  }

  /* Over with initial animation */

  /* Strength Meter */

  .strength-meter {
    display: flex;
    height: 20px;
    width: 100%;
  }
  .bar {
    margin-right: 5px;
    height: 100%;
    width: 25%;
    transition: box-shadow 500ms;
    box-shadow: inset 0px 20px #1f1f1f;
  }

  .bar-show {
    box-shadow: none;
  }
  .strengthbar-1 {
    background: linear-gradient(to right, red, orangered);
  }
  .strengthbar-2 {
    background: linear-gradient(to right, orangered, yellow);
  }
  .strengthbar-3 {
    background: linear-gradient(to right, yellow, yellowgreen);
  }
  .strengthbar-4 {
    background: linear-gradient(to right, yellowgreen, green);
  }

  /* toggle passoword show and hide */
  .toggle-password {
    position: absolute;
    cursor: pointer;
    font-size: 0.8rem;
    right: 0.25rem;
    bottom: 0.5rem;
  }

  button {
    margin-top: 2rem;
    margin-left: 12rem;
    padding: 10px 30px;
    font-weight: bold;
    border: 2px solid greenyellow;
    color: greenyellow;
    border-radius: 100px;
    background: green;
    transition: all 1000ms;
  }
  button:disabled {
    border-color: var(--text-color);
    color: var(--text-color);
    background: red;
  }
</style>

<!-- HTML HERE -->

<main>
  <form>
    <div class="field">
      <!-- Here Input Tag is before the table -->
      <input type="email" name="email" class="input" placeholder=" " />
      <label for="email" class="label">Enter Your Email:</label>
    </div>
    <div class="field">
      <!-- Here Input Tag is before the table -->
      <input
        type={showPassword ? 'text' : 'password'}
        name="email"
        class="input"
        placeholder=" "
        id="password"
        on:input={checkPassowrd} />
      <label for="password" class="label">Enter your Password:</label>
      <span
        class="toggle-password"
        on:mouseenter={() => (showPassword = true)}
        on:mouseleave={() => (showPassword = false)}>
        {showPassword ? '🙈' : '👁️'}
      </span>
    </div>

    <div class="strength-meter">
      <span class="bar strengthbar-1" class:bar-show={strength > 0} />
      <span class="bar strengthbar-2" class:bar-show={strength > 1} />
      <span class="bar strengthbar-3" class:bar-show={strength > 2} />
      <span class="bar strengthbar-4" class:bar-show={strength > 3} />
    </div>

    <ul style="list-style-type:none;">
      <li>{validations[0] ? '✅' : '❌'} Has atleast 5 Characters</li>
      <li>{validations[1] ? '✅' : '❌'} Contains a Capital Letter</li>
      <li>{validations[2] ? '✅' : '❌'} Contains a Number</li>
      <li>{validations[3] ? '✅' : '❌'} Contains a Special Chcaracter</li>
    </ul>

    <button disabled={strength < 4}>Login</button>
  </form>
</main>
