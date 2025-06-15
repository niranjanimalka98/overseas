<template>
    <section class="contact-info py-0 px-0 px-0-md px-0-lg px-5-xl py-5">
        <div class="row px-3 px-3-sm px-5-md px-5-lg px-0-xl pb-4">
            <div class="">
                <h1 class="contact-title" ref="contactTitle">How to reach us</h1>
                <p class="contact-subtitle" ref="contactSubtitle">
                    Lorem ipsum dolor sit amet, consetetur.
                </p>
            </div>
        </div>
        <div class="contact-container">
            <div class="contact-content px-3 px-3-sm px-5-md px-5-lg px-0-xl" ref="contactForm">
                <form @submit.prevent="submitForm" class="contact-form">
                    <div class="form-row">
                        <div class="form-group">
                            <label for="firstName">First Name *</label>
                            <input type="text" id="firstName" v-model="formData.firstName"
                                :class="{ error: errors.firstName && submitted }" required />
                            <span v-if="errors.firstName && submitted" class="error-message">{{
                                errors.firstName
                                }}</span>
                        </div>

                        <div class="form-group">
                            <label for="lastName">Last Name *</label>
                            <input type="text" id="lastName" v-model="formData.lastName"
                                :class="{ error: errors.lastName && submitted }" required />
                            <span v-if="errors.lastName && submitted" class="error-message">{{
                                errors.lastName
                                }}</span>
                        </div>
                    </div>

                    <div class="form-group">
                        <label for="email">Email *</label>
                        <input type="email" id="email" v-model="formData.email"
                            :class="{ error: errors.email && submitted }" required />
                        <span v-if="errors.email && submitted" class="error-message">{{ errors.email }}</span>
                    </div>

                    <div class="form-group">
                        <label for="telephone">Telephone</label>
                        <input type="tel" id="telephone" v-model="formData.telephone" />
                    </div>

                    <div class="form-group">
                        <label for="message">Message</label>
                        <textarea id="message" v-model="formData.message" rows="5"></textarea>
                    </div>

                    <p class="required-note">* required fields</p>
                    <div class="form-group checkbox-group">
                        <!-- <input
              type="checkbox"
              id="terms"
              v-model="formData.termsAccepted"
              :class="{ error: errors.terms && submitted }"
            /> -->
                        <Checkbox id="terms" v-model="formData.termsAccepted"
                            :class="{ error: errors.terms && submitted }">I agree to the <a href="#"
                                class="terms-link">Terms & Conditions</a></Checkbox>
                    </div>
                    <div>
                        <span v-if="errors.terms && submitted" class="error-message">{{ errors.terms }}</span>
                    </div>

                    <button type="submit" class="submit-button">SUBMIT</button>
                </form>
            </div>

            <div class="map-container" ref="contactMap">
                <div class="mapouter">
                    <div class="gmap_canvas">
                        <iframe
                            src="https://maps.google.com/maps?q=C.%20de%20Antonio%20C%C3%A1novas%20del%20Castillo%2C%2040%2C%2028914%20Legan%C3%A9s%2C%20Madrid%2C%20Spain&t=&z=15&ie=UTF8&iwloc=A&output=embed"
                            frameborder="0" scrolling="no" class="google-map-iframe" allowfullscreen></iframe>
                    </div>
                </div>
            </div>

            <div v-if="formSubmitted" class="success-message">
                <div class="success-content">
                    <h2>Thank you!</h2>
                    <p>Your message has been sent successfully.</p>
                    <button @click="resetForm" class="close-button">Close</button>
                </div>
            </div>
        </div>
    </section>
</template>

<script setup>
import { ref, reactive, onMounted } from 'vue'
import Checkbox from './UI/Checkbox.vue'
import { gsap } from 'gsap'

const contactTitle = ref(null)
const contactSubtitle = ref(null)
const contactForm = ref(null)
const contactMap = ref(null)

onMounted(() => {
    gsap.set([contactTitle.value, contactSubtitle.value, contactForm.value, contactMap.value], {
        opacity: 0,
        y: 80,
    })

    const observer = new IntersectionObserver(
        (entries) => {
            entries.forEach((entry) => {
                if (entry.isIntersecting) {
                    let el = entry.target

                    if (el === contactTitle.value) {
                        gsap.to(el, {
                            opacity: 1,
                            y: 0,
                            duration: 1.2,
                            ease: 'power4.out',
                        })
                    } else if (el === contactSubtitle.value) {
                        gsap.to(el, {
                            opacity: 1,
                            y: 0,
                            duration: 1.2,
                            delay: 0.2,
                            ease: 'power4.out',
                        })
                    } else if (el === contactForm.value) {
                        gsap.to(el, {
                            opacity: 1,
                            y: 0,
                            duration: 1.4,
                            delay: 0.3,
                            ease: 'power4.out',
                        })
                    } else if (el === contactMap.value) {
                        gsap.to(el, {
                            opacity: 1,
                            y: 0,
                            duration: 1.4,
                            delay: 0.4,
                            ease: 'power4.out',
                        })
                    }

                    observer.unobserve(el)
                }
            })
        },
        {
            threshold: 0.2,
        },
    )

    observer.observe(contactTitle.value)
    observer.observe(contactSubtitle.value)
    observer.observe(contactForm.value)
    observer.observe(contactMap.value)
})

const formData = reactive({
    firstName: '',
    lastName: '',
    email: '',
    telephone: '',
    message: '',
    termsAccepted: false,
})

const errors = reactive({
    firstName: '',
    lastName: '',
    email: '',
    terms: '',
})

const submitted = ref(false)
const formSubmitted = ref(false)

const validateForm = () => {
    let isValid = true

    errors.firstName = ''
    errors.lastName = ''
    errors.email = ''
    errors.terms = ''

    if (!formData.firstName.trim()) {
        errors.firstName = 'First name is required'
        isValid = false
    }

    if (!formData.lastName.trim()) {
        errors.lastName = 'Last name is required'
        isValid = false
    }

    if (!formData.email.trim()) {
        errors.email = 'Email is required'
        isValid = false
    } else if (!/^\S+@\S+\.\S+$/.test(formData.email)) {
        errors.email = 'Please enter a valid email address'
        isValid = false
    }

    if (!formData.termsAccepted) {
        errors.terms = 'You must agree to the Terms & Conditions'
        isValid = false
    }

    return isValid
}

const submitForm = () => {
    submitted.value = true

    if (validateForm()) {
        formSubmitted.value = true
    }
}

const resetForm = () => {
    formData.firstName = ''
    formData.lastName = ''
    formData.email = ''
    formData.telephone = ''
    formData.message = ''
    formData.termsAccepted = false
    submitted.value = false
    formSubmitted.value = false
}
</script>

<style scoped>
.contact-info {
    background-color: #000;
}

.contact-title {
    font-size: 1.7rem !important;
    font-weight: 600;
    color: #fff;
    margin-bottom: 0 !important;
}

.contact-container {
    display: flex;
    color: #fff;
    font-family:
        -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans',
        'Helvetica Neue', sans-serif;
    gap: 2rem;
    position: relative;
}

.contact-content {
    flex: 1;
    max-width: 500px;
}

.contact-title {
    font-size: 2.5rem;
    font-weight: 600;
    margin-bottom: 1rem;
}

.contact-subtitle {
    color: #b2b2b2;
}

.contact-description {
    color: #aaa;
    margin-bottom: 2rem;
}

.contact-form {
    display: flex;
    flex-direction: column;
    gap: 1.25rem;
}

.form-row {
    display: flex;
    gap: 1rem;
}

.form-group {
    flex: 1;
    display: flex;
    flex-direction: column;
}

label {
    margin-bottom: 0.5rem;
    font-size: 0.9rem;
    color: #ccc;
}

input,
textarea {
    padding: 0.75rem;
    background-color: #363636;
    border: none;
    border-radius: 4px;
    color: #fff;
    font-size: 1rem;
}

input.error,
textarea.error {
    border: 1px solid #ff4d4d;
}

.error-message {
    color: #ff4d4d;
    font-size: 0.8rem;
    margin-top: 0.25rem;
}

.required-note {
    font-size: 0.8rem;
    color: #aaa;
    margin-top: -0.5rem;
}

.checkbox-group {
    flex-direction: row;
    align-items: center;
    gap: 0.5rem;
}

.checkbox-group input {
    width: 20px;
    height: 20px;
}

.terms-link {
    color: #fff;
    text-decoration: underline;
}

.submit-button {
    background-color: #d4a017;
    color: #fff;
    border: none;
    border-radius: 4px;
    padding: 0.75rem;
    font-size: 1rem;
    font-weight: 600;
    cursor: pointer;
    transition: background-color 0.2s;
    margin-top: 1rem;
    width: 40%;
    align-self: end;
}

.submit-button:hover {
    background-color: #c19015;
}

.map-container {
    flex: 1;
    display: flex;
    flex-direction: column;
}

.mapouter {
    position: relative;
    text-align: right;
    height: -webkit-fill-available;
    width: 100%;
}

.gmap_canvas {
    overflow: hidden;
    background: none !important;
    height: 100%;
    width: 100%;
    position: relative;
}

.google-map-iframe {
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
}

.location-details {
    margin-top: 1.5rem;
    padding: 1rem;
    background-color: #111;
    border-radius: 4px;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.location-address {
    line-height: 1.5;
}

.location-address strong {
    color: #d4a017;
}

.directions-button {
    background-color: #d4a017;
    color: #fff;
    text-decoration: none;
    padding: 0.5rem 1rem;
    border-radius: 4px;
    font-weight: 500;
    transition: background-color 0.2s;
}

.directions-button:hover {
    background-color: #c19015;
}

.success-message {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.8);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 1000;
}

.success-content {
    background-color: #222;
    padding: 2rem;
    border-radius: 8px;
    text-align: center;
    max-width: 400px;
}

.success-content h2 {
    margin-bottom: 1rem;
}

.close-button {
    background-color: #d4a017;
    color: #fff;
    border: none;
    border-radius: 4px;
    padding: 0.5rem 1rem;
    margin-top: 1rem;
    cursor: pointer;
}

@media (max-width: 1024px) {
    .contact-container {
        flex-direction: column;
    }

    .contact-content {
        max-width: 100%;
    }

    .location-details {
        flex-direction: column;
        align-items: flex-start;
        gap: 1rem;
    }

    .directions-button {
        align-self: flex-start;
    }

    .google-map-iframe {
        height: 50vh !important;
    }
}

@media (max-width: 576px) {
    .form-row {
        flex-direction: column;
    }

    .google-map-iframe {
        height: 50vh !important;
    }

    .contact-info {
        padding-bottom: 0px !important;
    }

    .submit-button {
        align-self: center;
        width: 60%;
    }
}

@media (max-width: 1024px) {
    .contact-info {
        padding-bottom: 0px !important;
    }
}
</style>
