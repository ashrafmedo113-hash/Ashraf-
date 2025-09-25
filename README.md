import React from 'react';

const ServiceList = ({ services, user, onDelete, onEdit, onOrder, t }) => {
  if (!services.length) {
    return <p>{t.noServices}</p>;
  }

  return (
    <div className="service-list">
      <h3>{t.services}</h3>
      <ul>
        {services.map((service, idx) => (
          <li key={idx} style={{border: '1px solid #ddd', margin: '10px', padding: '10px'}}>
            <h4>{service.name}</h4>
            <p>{service.description}</p>
            <p>{t.price || 'السعر'}: {service.price}</p>
            {user?.role === 'admin' && (
              <>
                <button onClick={() => onEdit(idx)}>{t.editService}</button>
                <button onClick={() => onDelete(idx)} style={{marginLeft: 8, color: 'red'}}>{t.delete}</button>
              </>
            )}
            {user?.role === 'client' && (
              <button onClick={() => onOrder(service)}>{t.order}</button>
            )}
          </li>
        ))}
      </ul>
    </div>
  );
};

export default ServiceList;

import React, { useState } from 'react';
import Login from './components/Login';
import Register from './components/Register';
import Home from './components/Home';
import AdminDashboard from './components/AdminDashboard';
import AddService from './components/AddService';
import EditService from './components/EditService';
import LanguageSwitcher from './components/LanguageSwitcher';

function App() {
  const [page, setPage] = useState('login');
  const [user, setUser] = useState(null);
  const [services, setServices] = useState([]);
  const [editIdx, setEditIdx] = useState(null);
  const [language, setLanguage] = useState('ar'); // اللغة الافتراضية العربية

  // نصوص متعددة اللغات (للتجربة فقط - يمكنك توسعتها لاحقاً)
  const t = {
    ar: {
      welcome: "مرحبا بك في خدماتي",
      logout: "تسجيل الخروج",
      adminPanel: "لوحة تحكم المدير",
      addService: "إضافة خدمة جديدة",
      editService: "تعديل الخدمة",
      delete: "حذف",
      order: "طلب الخدمة",
      save: "حفظ",
      cancel: "إلغاء",
      back: "رجوع",
      noServices: "لا توجد خدمات مضافة بعد.",
      services: "الخدمات المتاحة:",
      username: "اسم المستخدم",
      email: "البريد الإلكتروني",
      password: "كلمة المرور",
      role: "نوع الحساب",
      client: "مستخدم",
      freelancer: "عامل",
      admin: "مدير",
      register: "إنشاء حساب جديد",
      login: "دخول",
      notHaveAccount: "ليس لديك حساب؟",
      chooseRole: "اختر نوع الحساب",
      price: "السعر"
    },
    en: {
      welcome: "Welcome to Khadimati",
      logout: "Logout",
      adminPanel: "Admin Dashboard",
      addService: "Add New Service",
      editService: "Edit Service",
      delete: "Delete",
      order: "Order Service",
      save: "Save",
      cancel: "Cancel",
      back: "Back",
      noServices: "No services added yet.",
      services: "Available Services:",
      username: "Username",
      email: "Email",
      password: "Password",
      role: "Account type",
      client: "Client",
      freelancer: "Freelancer",
      admin: "Admin",
      register: "Register",
      login: "Login",
      notHaveAccount: "Don't have an account?",
      chooseRole: "Choose account type",
      price: "Price"
    },
    fr: {
      welcome: "Bienvenue sur Khadimati",
      logout: "Déconnexion",
      adminPanel: "Tableau de bord admin",
      addService: "Ajouter un service",
      editService: "Modifier le service",
      delete: "Supprimer",
      order: "Commander le service",
      save: "Enregistrer",
      cancel: "Annuler",
      back: "Retour",
      noServices: "Aucun service ajouté.",
      services: "Services disponibles:",
      username: "Nom d'utilisateur",
      email: "Email",
      password: "Mot de passe",
      role: "Type de compte",
      client: "Client",
      freelancer: "Freelancer",
      admin: "Admin",
      register: "Créer un compte",
      login: "Connexion",
      notHaveAccount: "Pas de compte?",
      chooseRole: "Choisissez le type de compte",
      price: "Prix"
    },
    // أضف المزيد حسب الحاجة
  };

  // دوال التفاعل
  const handleLogin = (username, password) => {
    // حساب المدير الثابت
    if (username === 'admin') {
      setUser({ username, role: 'admin' });
    } else {
      // ابحث في المستخدمين المسجلين (يمكنك ربط قاعدة بيانات لاحقاً)
      setUser({ username, role: 'client' }); // افتراضي عميل
    }
    setPage('home');
  };

  const handleRegisterSubmit = (newUser) => {
    setUser(newUser);
    setPage('home');
  };

  const handleLogout = () => {
    setUser(null);
    setPage('login');
  };

  const handleAddService = (service) => {
    setServices([...services, service]);
    setPage('home');
  };

  const handleDeleteService = (idx) => {
    if (window.confirm(t[language].delete + "?")) {
      const newServices = [...services];
      newServices.splice(idx, 1);
      setServices(newServices);
    }
  };

  const handleEditService = (idx) => {
    setEditIdx(idx);
    setPage('editService');
  };

  const handleSaveEditService = (service) => {
    const newServices = [...services];
    newServices[editIdx] = service;
    setServices(newServices);
    setEditIdx(null);
    setPage('home');
  };

  const handleOrderService = (service) => {
    alert(`${t[language].order}: ${service.name}`);
  };

  return (
    <div style={{maxWidth: 600, margin: 'auto', padding: 20}}>
      <LanguageSwitcher currentLang={language} onChange={setLanguage} />
      {page === 'login' && (
        <Login
          onLogin={handleLogin}
          onRegister={() => setPage('register')}
          t={t[language]}
        />
      )}
      {page === 'register' && (
        <Register
          onRegisterSubmit={handleRegisterSubmit}
          t={t[language]}
        />
      )}
      {page === 'home' && (
        <Home
          user={user}
          services={services}
          onLogout={handleLogout}
          onAdmin={() => setPage('admin')}
          onDelete={handleDeleteService}
          onEdit={handleEditService}
          onOrder={handleOrderService}
          t={t[language]}
        />
      )}
      {page === 'admin' && (
        <AdminDashboard
          onAddService={() => setPage('addService')}
          onBack={() => setPage('home')}
          t={t[language]}
        />
      )}
      {page === 'addService' && (
        <AddService
          onAddService={handleAddService}
          onBack={() => setPage('admin')}
          t={t[language]}
        />
      )}
      {page === 'editService' && (
        <EditService
          service={services[editIdx]}
          onSave={handleSaveEditService}
          onCancel={() => { setEditIdx(null); setPage('home'); }}
          t={t[language]}
        />
      )}
    </div>
  );
}

export default App;
import React from 'react';

const languages = [
  { code: 'ar', name: 'العربية' },
  { code: 'en', name: 'English' },
  { code: 'fr', name: 'Français' },
  // يمكنك إضافة المزيد من اللغات هنا
];

const LanguageSwitcher = ({ currentLang, onChange }) => (
  <div style={{marginBottom: 20}}>
    <label htmlFor="language-select" style={{marginRight: 8}}>🌐</label>
    <select
      id="language-select"
      value={currentLang}
      onChange={e => onChange(e.target.value)}
    >
      {languages.map(lang => (
        <option key={lang.code} value={lang.code}>{lang.name}</option>
      ))}
    </select>
  </div>
);

export default LanguageSwitcher;
import React, { useState } from 'react';

const Login = ({ onLogin, onRegister, t }) => {
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');

  const handleLogin = (e) => {
    e.preventDefault();
    onLogin(username, password);
  };

  return (
    <div className="login-container">
      <h2>{t.login}</h2>
      <form onSubmit={handleLogin}>
        <div>
          <label>{t.username}</label>
          <input
            type="text"
            value={username}
            onChange={e => setUsername(e.target.value)}
            required
          />
        </div>
        <div>
          <label>{t.password}</label>
          <input
            type="password"
            value={password}
            onChange={e => setPassword(e.target.value)}
            required
          />
        </div>
        <button type="submit">{t.login}</button>
      </form>
      <p>
        {t.notHaveAccount}{' '}
        <button onClick={onRegister}>{t.register}</button>
      </p>
    </div>
  );
};

export default Login;
import React, { useState } from 'react';

const Register = ({ onRegisterSubmit, t }) => {
  const [username, setUsername] = useState('');
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [role, setRole] = useState('client'); // 'client' أو 'freelancer'

  const handleRegister = (e) => {
    e.preventDefault();
    onRegisterSubmit({ username, email, password, role });
  };

  return (
    <div className="register-container">
      <h2>{t.register}</h2>
      <form onSubmit={handleRegister}>
        <div>
          <label>{t.username}</label>
          <input
            type="text"
            value={username}
            onChange={e => setUsername(e.target.value)}
            required
          />
        </div>
        <div>
          <label>{t.email}</label>
          <input
            type="email"
            value={email}
            onChange={e => setEmail(e.target.value)}
            required
          />
        </div>
        <div>
          <label>{t.password}</label>
          <input
            type="password"
            value={password}
            onChange={e => setPassword(e.target.value)}
            required
          />
        </div>
        <div>
          <label>{t.chooseRole}</label>
          <select value={role} onChange={e => setRole(e.target.value)}>
            <option value="client">{t.client}</option>
            <option value="freelancer">{t.freelancer}</option>
          </select>
        </div>
        <button type="submit">{t.register}</button>
      </form>
    </div>
  );
};

export default Register;
import React from 'react';

const ServiceList = ({ services, user, onDelete, onEdit, onOrder, t }) => {
  if (!services.length) {
    return <p>{t.noServices}</p>;
  }

  return (
    <div className="service-list">
      <h3>{t.services}</h3>
      <ul>
        {services.map((service, idx) => (
          <li key={idx} style={{border: '1px solid #ddd', margin: '10px', padding: '10px'}}>
            <h4>{service.name}</h4>
            <p>{service.description}</p>
            <p>{t.price}: {service.price}</p>
            {user?.role === 'admin' && (
              <>
                <button onClick={() => onEdit(idx)}>{t.editService}</button>
                <button onClick={() => onDelete(idx)} style={{marginLeft: 8, color: 'red'}}>{t.delete}</button>
              </>
            )}
            {user?.role === 'client' && (
              <button onClick={() => onOrder(service)}>{t.order}</button>
            )}
          </li>
        ))}
      </ul>
    </div>
  );
};

export default ServiceList;
import React from 'react';

const AdminDashboard = ({ onAddService, onBack, t }) => (
  <div className="admin-dashboard">
    <h2>{t.adminPanel}</h2>
    <button onClick={onAddService}>{t.addService}</button>
    <button onClick={onBack} style={{marginLeft: 10}}>{t.back}</button>
  </div>
);

export default AdminDashboard;
import React, { useState } from 'react';

const AddService = ({ onAddService, onBack, t }) => {
  const [name, setName] = useState('');
  const [description, setDescription] = useState('');
  const [price, setPrice] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    if (name && description && price) {
      onAddService({ name, description, price });
      setName('');
      setDescription('');
      setPrice('');
    }
  };

  return (
    <div className="add-service-container">
      <h2>{t.addService}</h2>
      <form onSubmit={handleSubmit}>
        <div>
          <label>{t.services}</label>
          <input
            type="text"
            value={name}
            onChange={e => setName(e.target.value)}
            required
          />
        </div>
        <div>
          <label>{t.editService}</label>
          <textarea
            value={description}
            onChange={e => setDescription(e.target.value)}
            required
          />
        </div>
        <div>
          <label>{t.price}</label>
          <input
            type="number"
            value={price}
            onChange={e => setPrice(e.target.value)}
            required
          />
        </div>
        <button type="submit">{t.addService}</button>
        <button type="button" onClick={onBack} style={{marginLeft: 10}}>{t.back}</button>
      </form>
    </div>
  );
};

export default AddService;
import React, { useState } from 'react';

const EditService = ({ service, onSave, onCancel, t }) => {
  const [name, setName] = useState(service.name);
  const [description, setDescription] = useState(service.description);
  const [price, setPrice] = useState(service.price);

  const handleSubmit = (e) => {
    e.preventDefault();
    onSave({ name, description, price });
  };

  return (
    <div className="edit-service-container">
      <h2>{t.editService}</h2>
      <form onSubmit={handleSubmit}>
        <div>
          <label>{t.services}</label>
          <input
            type="text"
            value={name}
            onChange={e => setName(e.target.value)}
            required
          />
        </div>
        <div>
          <label>{t.editService}</label>
          <textarea
            value={description}
            onChange={e => setDescription(e.target.value)}
            required
          />
        </div>
        <div>
          <label>{t.price}</label>
          <input
            type="number"
            value={price}
            onChange={e => setPrice(e.target.value)}
            required
          />
        </div>
        <button type="submit">{t.save}</button>
        <button type="button" onClick={onCancel} style={{marginLeft: 10}}>{t.cancel}</button>
      </form>
    </div>
  );
};

export default EditService;
