<script>
  import LinkButton from './LinkButton.svelte'
  import Text from './Text.svelte'
  import Button from './Button.svelte'
  import Input from './Input.svelte'

  export let supabaseClient
  export let view
  export let setView

  let error = '', message = '', loading = false, email = '', password = '', name = ''

  async function submit() {
    error = ''
    message = ''
    loading = true

    if (view == 'sign_up') {
      const { data: authData, error: signUpError } = await supabaseClient.auth.signUp({
        email, 
        password,
        data: { name }
      })

      if (signUpError) error = signUpError.message
      if (authData) message = "Revisa tu bandeja de entrada."
    } else if (view == 'sign_in') {
      const { error: signInError } = await supabaseClient.auth.signIn({
        email, password
      })

      if (signInError) error = signInError.message
    }

    loading = false
  }
</script>

<form on:submit|preventDefault={submit}>
  <Input name="email" type="email" label="Dirección de correo" icon="mail" bind:value={email}/>
  <Input name="password" type="password" label="Contraseña" icon="key" bind:value={password}/>
  <Input name="name" type="text" label="Nombre Completo" icon="user" bind:value={name}/>


  {#if view == 'sign_up'}
    <Button block primary size="large" {loading} icon="inbox">Registro</Button>
    <div class="links">
      <LinkButton on:click={() => setView('magic_link')}>Ingresar con magic link</LinkButton>
      <LinkButton on:click={() => setView('sign_in')}>Tienes una cuenta? Inicia sesión</LinkButton>
    </div>
  {:else}
    <Button block primary size="large" {loading} icon="inbox">Inicia sesión</Button>
    <div class="links">
      <LinkButton on:click={() => setView('sign_up')}>No tienes una cuenta? Regístrate</LinkButton>
    </div>
  {/if}

  {#if message}
    <Text>{message}</Text>
  {/if}

  {#if error}
    <Text type="danger">{error}</Text>
  {/if}
</form>

<style>
  form {
    display: flex;
    flex-direction: column;
  }

  .links {
    display: flex;
    flex-direction: column;
    margin: 1rem 0;
    gap: 0.5rem;
  }
</style>
