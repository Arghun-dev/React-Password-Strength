# React-Password-Strength

### step 1

```js
$. npm i react-password-strength
```

### step 2

```js
constructor(props) {
    super(props);

    this.state = {
        newPass: '',
        confirmNewPass: '',
        error: '',
        score: 0,
        isValidPass: null,
        passLength: 0,
    }
}

// HERE CHANGE THE STATE OF THE LENGTH AND COLOR
changeCallback = (state) => {
    this.setState({ passLength: state.password.length })
}

// HERE I CLEAR THE LENGTH AND COLOR
clear = () => this.ReactPasswordStrength.clear()

// HERE FIRST I EXAMINE THE PASSWORDS THEN I SEND THE FORM
changePassword = e => {
  e.preventDefault();
  if (this.state.newPass !== this.state.confirmNewPass) {
      this.setState({ error: 'کلمه عبور با تکرار ان مطابقت ندارد' })
  } else if (this.state.newPass === this.state.confirmNewPass && !this.state.isValidPass && this.state.score < 4) {
      this.setState({ error: 'نام کاربری امن تری انتخاب کنید' })
  } else if (this.state.newPass === this.state.confirmNewPass && this.state.score >= 4 && this.state.isValidPass) {
      if (this.props.activationCodeRequest[0]?.UserId && this.props.activationCodeRequest[0]?.ActivationId) {
          this.props.userPasswordForget_Reset(this.props.activationCodeRequest[0].UserId, this.props.activationCodeRequest[0].ActivationId, this.state.newPass)
          this.props.forgetPasswordAction()
      }
  }
}
    
render() {

  const newPassInputProps = {
      // placeholder: "Try a password...",
      autoFocus: true,
      className: 'another-input-prop-class-name',
      value: this.state.newPass,
      name: 'newPass'
  };

  const newPassConfirmInputProps = {
      autoFocus: true,
      className: 'another-input-prop-class-name',
      value: this.state.confirmNewPass,
      name: 'confirmNewPass'
  }
  
  return (
    <form onSubmit={this.changePassword} style={{ display: 'flex', flexDirection: 'column', justifyContent: 'space-around', alignItems: 'center', marginTop: '2rem' }}>
      <label className='font-weight-bold align-self-start'>کلمه عبور</label>
      <ReactPasswordStrength
          changeCallback={val => this.setState({ newPass: val.password, isValidPass: val.isValid, score: val.score })}
          className="CustomInput"
          minLength={6}
          autoFocus={true}
          scoreWords={['ضعیف', 'نسبتا خوب', 'خوب', 'قوی', 'عالی']}
          tooShortWord='خیلی ضعیف'
          inputProps={{ ...newPassInputProps, id: 'newPass' }}
      />
      <label className='font-weight-bold align-self-start'>تکرار کلمه عبور</label>
      <ReactPasswordStrength
          changeCallback={val => this.setState({ confirmNewPass: val.password })}
          className="CustomInput"
          minLength={6}
          autoFocus={true}
          scoreWords={['ضعیف', 'نسبتا خوب', 'خوب', 'قوی', 'عالی']}
          tooShortWord='خیلی ضعیف'
          inputProps={{ ...newPassConfirmInputProps, id: 'newPassConfirm' }}
      />
      <p className='text-danger' style={{ marginBottom: '-.5rem' }}>{this.state.error}</p>
      <Button style={{ marginBottom: '1.5rem', marginTop: '1rem' }} color='primary' size='sm'>تغییر رمز</Button>
  </form>
  )
}
```

**As you can see ReactPasswordStrength gives me back an object inside `changeCallback`, and I called that `val` and inside of this object I have access to 3 parameters.**

val object:
```js
val = {
  isActive: true or false,
  score: 0 or 1 or 2 or 3 or 4,
  password: the password that user inputs
}
```
