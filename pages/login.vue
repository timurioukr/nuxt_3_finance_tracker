<template>
  <UCard v-if="!success">
    <template #header>
      Sign in Finance Tracker
    </template>

    <form @submit.prevent="handleLogin">
      <UFormGroup label="Email" name="email" class="mb-4" :required="true" help="Tou will receive an email with the confirmation link">
        <UInput
          v-model="email"
          required
          type="email"
          placeholder="Enter your email"
        />
      </UFormGroup>

      <UButton type="submit" variant="solid" color="black" :loading="pending" :disabled="pending">Sign in</UButton>
    </form>
  </UCard>

  <UCard v-else>
    <template #header>
      Email has been sent
    </template>

    <div class="text-center">
      <p class="mb-4">We have sent an email to <strong>{{ email }}</strong> with a link to restore</p>
      <p>
        <strong>
          Important 
        </strong>
        The link will expire in 5 minutes.
      </p>
    </div>

  </UCard>
</template>

<script setup>
const success = ref(false)
const email = ref('')
const pending = ref(false)
const toast = useToast()
const supabase = useSupabaseClient()

useRedirectFromAuth()

const handleLogin = async () => {
  pending.value = true

  try {
    const { data, error } = await supabase.auth.signInWithOtp({
      email: email.value,
      options: {
        emailRedirectTo: 'http://localhost:3000/confirm'
      }
    })

    if (error) {
      toast.add({
        title: 'Error authenticating',
        icon: 'i-heroicons-exlamation-circle',
        description: error.message,
        color: 'red'
      })
    } else {
      success.value = true
    }


  } finally {
    pending.value = false
  }
  // try {
  //   const { data, error } = await supabase.auth.signIn({
  //     email: email.value,
  //   })
    // if (error) {
    //   toast.error(error.message)
    // } else {
    //   success.value = true
    // }
  // } catch (error) {
  //   toast.error(error.message)
  // } finally {
  //   pending.value = false
  // }
}
</script>