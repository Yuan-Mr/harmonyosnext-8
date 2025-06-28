👋 Hello everyone! Today, let's explore how to implement account linking with ArkTS (API 12) in HarmonyOS app development. For social apps, games, or utility tools, the account system is a key part of user experience. With flexible account linking, users can log in via mobile phone, email, Huawei Account, etc., and freely bind/unbind accounts for easier management!  


### 🌟 Why Implement Account Linking?  
Imagine a user registers with a mobile number but later wants to switch to email login without losing data—account linking solves this! By linking multiple authentication methods, users can log in via any method, and the system recognizes them as the same account with seamless data synchronization. Developers can also manage user behavior via a unified UID, boosting operational efficiency.  


### 📋 Prerequisites  
- **Service Activation**: Enable *Authentication Service* in the AGC console.  
- **SDK Integration**: Add the `@hw-agconnect/auth` package to your project.  
- **App Configuration**: Ensure supported authentication methods (e.g., mobile, email, Huawei Account) are configured.  


### 🔗 Three Ways to Link Accounts (with Code)  
#### 1️⃣ Link a Mobile Number  
When a user logged in via another method (e.g., email) wants to bind a mobile number:  
```typescript  
import auth from '@hw-agconnect/auth';  
import { hilog } from '@kit.PerformanceAnalysisKit';  

auth.getCurrentUser().then((user: AuthUser | null) => {  
  user!.link({  
    kind: "phone",  
    phoneNumber: "180****1485",  
    countryCode: "86",  
    verifyCode: "123456" // Obtain from SMS in real development  
  }).then(() => {  
    hilog.info(0x0000, 'AuthDemo', 'Mobile number linked successfully!');  
  }).catch((error: Error) => {  
    hilog.error(0x0000, 'AuthDemo', `Linking failed: ${error.message}`);  
  });  
});  
```  
**Note**: Call the SMS verification API to get the code first.  

#### 2️⃣ Link an Email  
When a user wants to bind an email as an alternative login:  
```typescript  
user!.link({  
  kind: "email",  
  email: "user@example.com",  
  password: "SecurePassword123!", // Optional (if password is set)  
  verifyCode: "7890" // Obtain from the email verification code  
}).then(() => {  
  hilog.info(0x0000, 'AuthDemo', 'Email linked successfully!');  
});  
```  

#### 3️⃣ Link a Huawei Account  
One-click binding for Huawei Accounts, ideal for ecosystem apps:  
```typescript  
user!.link({ kind: "hwid" })  
  .then(() => hilog.info(0x0000, 'AuthDemo', 'Huawei Account linked successfully!'));  
```  


### ⚠️ Pitfall Prevention  
- **Uniqueness Rule**: Each authentication method can only bind to one account (e.g., cannot bind two mobile numbers).  
- **Sensitive Ops Protection**: Actions like password changes or unbinding must be done within 5 minutes of login; reauthenticate if timed out.  
- **Retain One Method**: The last authentication method can't be unbound to prevent account loss.  


### 🔓 How to Unbind an Account  
When a user wants to unbind a method (ensure at least one remains):  
```typescript  
// Unbind mobile number  
auth.getCurrentUser().then(user => {  
  user.unlink("phone")  
    .then(() => hilog.info(0x0000, 'AuthDemo', 'Mobile number unbound!'));  
});  
```  


### 💡 Advanced Tips  
- **Unified User ID**: Use `user.getUid()` to get a unique ID for consistent user identification.  
- **Error Handling**: Wrap sensitive ops in try-catch with friendly messages:  
  ```typescript  
  catch((error: Error) => {  
    if (error.code === '2032') {  
      alert('Invalid verification code, request again!');  
    }  
  });  
  ```  
- **Multi-Device Sync**: Linking/unbinding syncs real-time across all logged-in devices.  


### 🎯 Best Practices  
- **User Upgrade**: When anonymous users convert to formal accounts, bind mobile/email to retain data.  
- **Duplicate Account Merge**: Prompt linking when the system detects the same user registered via different methods.  
- **Security Enhancement**: Guide users to bind a second verification method as a backup.  


### 🚀 Conclusion  
ArkTS account linking lets developers build flexible, secure user systems with ease. Whether enhancing UX or optimizing backend management, it's essential for HarmonyOS development. Give it a try! Share your issues or stories in the comments.  

✨ **互动时间**: What tricky account system issues have you faced in development? Share your experiences!  

Hope this guide serves as a practical handbook for your HarmonyOS journey. Stay tuned! 👨💻🚀
