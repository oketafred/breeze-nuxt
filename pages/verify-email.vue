<template>
    <div>
        <div class="mb-4 text-sm text-gray-600">
            Thanks for signing up! Before getting started, could you verify your email address by clicking on the link we just emailed to you? If you didn't receive the email, we will gladly send you another.
        </div>

        <div class="mb-4 font-medium text-sm text-green-600" v-if="verificationLinkSent">
            A new verification link has been sent to the email address you provided during registration.
        </div>

        <form @submit.prevent="submit">
            <div class="mt-4 flex items-center justify-between">
                <BreezeButton :class="{ 'opacity-25': form.processing }" :disabled="form.processing">
                    Resend Verification Email
                </BreezeButton>

                <button @click="logout()" class="underline text-sm text-gray-600 hover:text-gray-900">Log Out</button>
            </div>
        </form>
    </div>
</template>

<script>
import BreezeButton from '@/components/button.vue'
export default {
    layout: 'guest',
    components: {
        BreezeButton,
    },
    data() {
        return {
            form: {
                status: '',
                processing: false
            }
        }
    },
    methods: {
        submit() {
            this.processing = true

            this.$axios.get('sanctum/csrf-cookie').then(() => {
                this.$axios.$post('/email/verification-notification').then(() => {
                    this.status = 'verification-link-sent'

                    this.processing = false
                })
            })
        },
    },
    computed: {
        verificationLinkSent() {
            return this.status === 'verification-link-sent';
        }
    }
}
</script>