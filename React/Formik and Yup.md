**Formik** is a node module that simplifies the process of creating forms within the app. It is an alternative instead of handling your own state variables for each form field, etc.

Quickstart:
```TSX
import { TextInput, TouchableOpacity } from 'react-native';
import { useFormik } from 'formik';
import * as Yup from 'yup'

const LoginForm = () => {
	const formik = useFormik({
		initialValues: getInitialValues(),
		validationSchema: Yup.object(getValidationSchema()),
		validationOnChange: false,
		onSubmit: (formValueObj) => {
			console.log('Sending form...');
			console.log(formValueObj);
		}
	});
	
	return (
		<>
			<TextInput 
				placeholder='Enter you username' 
				value={formik.values.username}
				onChangeText={(text) => formik.setFieldValue("username", text)} 
			/>
			<TextInput 
				placeholder='Enter your password' 
				value={formik.values.password}
				onChangeText={(text) => formik.setFieldValue("password", text)} 
			/>
			<TouchableOpacity
				onPress={formik.handleSubmit} 
			/>
			<Text>{formik.errors.username || formik.errors.password}</Text>
		</>
	) 
}

const getInitialValues = () => {
	return {
		username: '',
		password: ''
	}
}

const getValidationSchema = () => {
	return {
		username: Yup.string().required('Specifying an username is mandatory'),
		password: Yup.string().required('Specifying the password is mandatory')
	}
}

export default function LoginForm;
```