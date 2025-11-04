# SMTP Email Setup Instructions for CRBFTN Website

## 🔧 Complete SMTP Configuration with Netlify Functions

Your CRBFTN website now uses a robust serverless SMTP solution for guaranteed email delivery. This replaces the previous Netlify form notifications with a custom function that sends emails via SMTP.

## 📧 What Changed

### New Email System:
- **Serverless Function**: `/.netlify/functions/send-email` handles all email sending
- **SMTP Delivery**: Uses nodemailer with Gmail SMTP for reliable delivery
- **Dual Emails**: Sends both business notifications and customer confirmations
- **Rich HTML Templates**: Beautiful branded email templates with cart details

### Email Types:
1. **Contact Form**:
   - Business notification → `email.crabfontain@gmail.com`
   - Customer thank you → customer's email address

2. **Quote Requests**:
   - Business notification → `email.crabfontain@gmail.com`
   - Customer confirmation → customer's email address

## 🚀 Setup Instructions

### Step 1: Gmail App Password Setup

Since you're using Gmail for SMTP, you need to create an App Password:

1. **Go to Google Account Settings**: https://myaccount.google.com/
2. **Navigate to**: Security → 2-Step Verification (enable if not already)
3. **Go to**: Security → App passwords
4. **Create App Password**:
   - Select app: "Mail"
   - Select device: "Other" → enter "CRBFTN Website"
   - **Copy the 16-character password** (e.g., `abcd efgh ijkl mnop`)

### Step 2: Netlify Environment Variables

In your Netlify dashboard, go to: **Site Settings → Environment Variables**

Add these variables:

```
EMAIL_HOST = smtp.gmail.com
EMAIL_PORT = 587
EMAIL_USER = email.crabfontain@gmail.com
EMAIL_PASS = [your-16-character-app-password]
EMAIL_TO = email.crabfontain@gmail.com
```

**Important**: 
- Use the **App Password** (not your regular Gmail password) for `EMAIL_PASS`
- Set `EMAIL_USER` to your Gmail address
- Set `EMAIL_TO` to where you want business notifications sent

### Step 3: Deploy Your Site

After setting environment variables:
1. **Trigger a new deployment** in Netlify (or push changes to your repository)
2. Netlify will install the `nodemailer` dependency and deploy the function

### Step 4: Test Email Delivery

1. **Contact Form Test**:
   - Go to `/contact.html`
   - Fill out and submit the contact form
   - Check both your business email and the test email address

2. **Quote Request Test**:
   - Add items to cart
   - Click "Request Quote"
   - Fill in the email modal and submit
   - Check both business notification and customer confirmation emails

## 🔍 Troubleshooting

### If emails aren't being sent:

1. **Check Netlify Function Logs**:
   - Go to Netlify Dashboard → Functions
   - Click on `send-email` function
   - Check the logs for any errors

2. **Verify Environment Variables**:
   - Ensure all 5 SMTP variables are set correctly
   - Double-check the App Password (no spaces)
   - Confirm EMAIL_USER matches your Gmail address

3. **Gmail Security Settings**:
   - Ensure 2-Step Verification is enabled
   - Verify the App Password is correct
   - Check if Gmail is blocking the connection

4. **Test Function Directly**:
   - You can test the function in Netlify's Function logs
   - Look for specific error messages about SMTP connection

### Common Error Messages:

**"Invalid login"**: App Password is incorrect or 2FA not enabled
**"Connection timeout"**: Network/firewall issue or wrong SMTP settings
**"SMTP configuration incomplete"**: Missing environment variables

## 📋 Email Template Features

### Business Notifications Include:
- Customer contact information
- Complete cart details with sizes and quantities
- Total amounts and item counts
- Quote IDs and timestamps
- Professional CRBFTN branding

### Customer Confirmations Include:
- Personalized greeting with customer name
- Order summary with requested items
- Next steps and timeline expectations
- Professional CRBFTN branding
- Contact information for questions

## 🛡️ Security & Best Practices

### Environment Variables Security:
- ✅ SMTP credentials stored securely in Netlify
- ✅ No passwords exposed in source code
- ✅ App Password isolates website access from main account

### Email Deliverability:
- ✅ SMTP delivery more reliable than form notifications
- ✅ Professional sender reputation via Gmail
- ✅ Rich HTML templates improve engagement
- ✅ Proper error handling and logging

## 📞 Testing Checklist

After setup, verify these work:

- [ ] Contact form sends business notification to `email.crabfontain@gmail.com`
- [ ] Contact form sends thank you email to customer
- [ ] Quote request sends business notification with cart details
- [ ] Quote request sends confirmation to customer email
- [ ] Emails arrive within 1-2 minutes
- [ ] HTML formatting displays correctly
- [ ] All customer information appears in business emails
- [ ] Cart items and totals are accurate in quote emails

## 🎯 Expected Results

### Successful Setup Shows:
1. **Form submissions trigger emails** within 1-2 minutes
2. **Business gets notifications** at `email.crabfontain@gmail.com`
3. **Customers receive confirmations** at their provided email
4. **Rich HTML emails** with CRBFTN branding and complete details
5. **No more "email not working" issues**

### If Still Having Issues:

1. **Check spam/junk folders** for both business and test emails
2. **Verify Netlify Function deployed** correctly (check Functions tab)
3. **Review Function logs** for specific error messages
4. **Test with a different email provider** for customer confirmations
5. **Contact me** with specific error messages from Netlify logs

Your CRBFTN website now has enterprise-grade email delivery! 🎉