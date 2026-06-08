<script setup>
	import { Notyf } from 'notyf';
	import { ref, onMounted, onBeforeUnmount } from 'vue';

	const notyf = new Notyf();

	const name = ref("");
	const email = ref("");
	const message = ref("");
	const isLoading = ref(false);

	// 1. Web3Forms Access Key
	const WEB3FORMS_ACCESS_KEY = "91c0b0e7-58c2-48d8-a99c-f04255e380e5"; 

	// Email subject that will appear when a form submission is received.
	const subject = "New message from Gladys Ramos Portfolio";

	/* reCAPTCHA Integration Setup */
	// 2. Google reCAPTCHA V2 Site Key updated with your personal verified key
	const SITE_KEY = '6LdfC0MfAAAAAF963m998U0ky9snF_1E_z8isY6v';  

	const recaptchaContainer = ref(null);
	const recaptchaWidgetId = ref(null);
	const recaptchaToken = ref('');

	function onRecaptchaSuccess(token) {
		recaptchaToken.value = token;
	}

	function onRecaptchaExpired() {
		recaptchaToken.value = '';
	}

	function renderRecaptcha() {
		if (!window.grecaptcha || !window.grecaptcha.enterprise) {
			console.error('reCAPTCHA Enterprise not loaded');
			return;
		}

		recaptchaWidgetId.value = window.grecaptcha.enterprise.render(recaptchaContainer.value, {
			sitekey: SITE_KEY,
			size: 'normal',
			callback: onRecaptchaSuccess,
			'expired-callback': onRecaptchaExpired,
		});
	}

	function resetRecaptcha() {
		if (recaptchaWidgetId.value !== null) {
			window.grecaptcha.enterprise.reset(recaptchaWidgetId.value);
			recaptchaToken.value = '';
		}
	}

	// The submitForm() function handles the contact form submission.
	const submitForm = async () => {
		// Validation check: ensure token exists from reCAPTCHA verification
		if (!recaptchaToken