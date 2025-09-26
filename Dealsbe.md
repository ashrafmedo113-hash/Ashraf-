import React, { useState } from 'react';

const languages = [
  { code: 'ar', name: 'العربية' },
  { code: 'en', name: 'English' },
  { code: 'fr', name: 'Français' },
  // أضف المزيد حسب الحاجة
];

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

function App() {
  const [page, setPage] = useState('login');
  const [user, setUser] = useState(null);
  const [services, setServices] = useState([]);
  const [editIdx, setEditIdx] = useState(null);
  const [language, setLanguage] = useState('ar');

  // دوال التفاعل
  const handleLogin = (username, password) => {
    if (username === 'admin') {
      setUser({ username, role: 'admin' });
    } else {
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

  // مكون اختيار اللغة
  const LanguageSwitcher = () => (
    <div style={{marginBottom: 20}}>
      <label htmlFor="language-select" style={{marginRight: 8}}>🌐</label>
      <select
        id="language-select"
        value={language}
        onChange={e => setLanguage(e.target.value)}
      >
        {languages.map(lang => (
          <option key={lang.code} value={lang.code}>{lang.name}</option>
        ))}
      </select>
    </div>
  );

  // صفحة تسجيل الدخول
  const Login = () => {
    const [username, setUsername] = useState('');
    const [password, setPassword] = useState('');
    const handleLoginSubmit = (e) => {
      e.preventDefault();
      handleLogin(username, password);
    };
    return (
      <div className="login-container">
        <h2>{t[language].login}</h2>
        <form onSubmit={handleLoginSubmit}>
          <div>
            <label>{t[language].username}</label>
            <input
              type="text"
              value={username}
              onChange={e => setUsername(e.target.value)}
              required
            />
          </div>
          <div>
            <label>{t[language].password}</label>
            <input
              type="password"
              value={password}
              onChange={e => setPassword(e.target.value)}
              required
            />
          </div>
          <button type="submit">{t[language].login}</button>
        </form>
        <p>
          {t[language].notHaveAccount}{' '}
          <button onClick={() => setPage('register')}>{t[language].register}</button>
        </p>
      </div>
    );
  };

  // صفحة تسجيل حساب جديد
  const Register = () => {
    const [username, setUsername] = useState('');
    const [email, setEmail] = useState('');
    const [password, setPassword] = useState('');
    const [role, setRole] = useState('client');

    const handleSubmit = (e) => {
      e.preventDefault();
      handleRegisterSubmit({ username, email, password, role });
    };

    return (
      <div className="register-container">
        <h2>{t[language].register}</h2>
        <form onSubmit={handleSubmit}>
          <div>
            <label>{t[language].username}</label>
            <input
              type="text"
              value={username}
              onChange={e => setUsername(e.target.value)}
              required
            />
          </div>
          <div>
            <label>{t[language].email}</label>
            <input
              type="email"
              value={email}
              onChange={e => setEmail(e.target.value)}
              required
            />
          </div>
          <div>
            <label>{t[language].password}</label>
            <input
              type="password"
              value={password}
              onChange={e => setPassword(e.target.value)}
              required
            />
          </div>
          <div>
            <label>{t[language].chooseRole}</label>
            <select value={role} onChange={e => setRole(e.target.value)}>
              <option value="client">{t[language].client}</option>
              <option value="freelancer">{t[language].freelancer}</option>
            </select>
          </div>
          <button type="submit">{t[language].register}</button>
        </form>
      </div>
    );
  };

  // قائمة الخدمات
  const ServiceList = () => {
    if (!services.length) {
      return <p>{t[language].noServices}</p>;
    }
    return (
      <div className="service-list">
        <h3>{t[language].services}</h3>
        <ul style={{listStyle: 'none', padding: 0}}>
          {services.map((service, idx) => (
            <li key={idx} style={{border: '1px solid #ddd', margin: '10px', padding: '10px'}}>
              <h4>{service.name}</h4>
              <p>{service.description}</p>
              <p>{t[language].price}: {service.price}</p>
              {user?.role === 'admin' && (
                <>
                  <button onClick={() => handleEditService(idx)}>{t[language].editService}</button>
                  <button onClick={() => handleDeleteService(idx)} style={{marginLeft: 8, color: 'red'}}>{t[language].delete}</button>
                </>
              )}
              {user?.role === 'client' && (
                <button onClick={() => handleOrderService(service)}>{t[language].order}</button>
              )}
            </li>
          ))}
        </ul>
      </div>
    );
  };

  // الصفحة الرئيسية
  const Home = () => (
    <div className="home-container">
      <h2>{t[language].welcome} {user?.username}!</h2>
      <button onClick={handleLogout}>{t[language].logout}</button>
      {user?.role === 'admin' && (
        <button onClick={() => setPage('admin')} style={{marginLeft: 10}}>{t[language].adminPanel}</button>
      )}
      <ServiceList />
    </div>
  );

  // لوحة المدير
  const AdminDashboard = () => (
    <div className="admin-dashboard">
      <h2>{t[language].adminPanel}</h2>
      <button onClick={() => setPage('addService')}>{t[language].addService}</button>
      <button onClick={() => setPage('home')} style={{marginLeft: 10}}>{t[language].back}</button>
    </div>
  );

  // إضافة خدمة جديدة
  const AddService = () => {
    const [name, setName] = useState('');
    const [description, setDescription] = useState('');
    const [price, setPrice] = useState('');

    const handleSubmit = (e) => {
      e.preventDefault();
      if (name && description && price) {
        handleAddService({ name, description, price });
        setName('');
        setDescription('');
        setPrice('');
      }
    };

    return (
      <div className="add-service-container">
        <h2>{t[language].addService}</h2>
        <form onSubmit={handleSubmit}>
          <div>
            <label>{t[language].services}</label>
            <input
              type="text"
              value={name}
              onChange={e => setName(e.target.value)}
              required
            />
          </div>
          <div>
            <label>{t[language].editService}</label>
            <textarea
              value={description}
              onChange={e => setDescription(e.target.value)}
              required
            />
          </div>
          <div>
            <label>{t[language].price}</label>
            <input
              type="number"
              value={price}
              onChange={e => setPrice(e.target.value)}
              required
            />
          </div>
          <button type="submit">{t[language].addService}</button>
          <button type="button" onClick={() => setPage('admin')} style={{marginLeft: 10}}>{t[language].back}</button>
        </form>
      </div>
    );
  };

  // تعديل خدمة
  const EditService = () => {
    const [name, setName] = useState(services[editIdx].name);
    const [description, setDescription] = useState(services[editIdx].description);
    const [price, setPrice] = useState(services[editIdx].price);

    const handleSubmit = (e) => {
      e.preventDefault();
      handleSaveEditService({ name, description, price });
    };

    return (
      <div className="edit-service-container">
        <h2>{t[language].editService}</h2>
        <form onSubmit={handleSubmit}>
          <div>
            <label>{t[language].services}</label>
            <input
              type="text"
              value={name}
              onChange={e => setName(e.target.value)}
              required
            />
          </div>
          <div>
            <label>{t[language].editService}</label>
            <textarea
              value={description}
              onChange={e => setDescription(e.target.value)}
              required
            />
          </div>
          <div>
            <label>{t[language].price}</label>
            <input
              type="number"
              value={price}
              onChange={e => setPrice(e.target.value)}
              required
            />
          </div>
          <button type="submit">{t[language].save}</button>
          <button type="button" onClick={() => { setEditIdx(null); setPage('home'); }} style={{marginLeft: 10}}>{t[language].cancel}</button>
        </form>
      </div>
    );
  };

  // ترتيب الصفحات
  return (
    <div style={{maxWidth: 600, margin: 'auto', padding: 20}}>
      <LanguageSwitcher />
      {page === 'login' && <Login />}
      {page === 'register' && <Register />}
      {page === 'home' && <Home />}
      {page === 'admin' && <AdminDashboard />}
      {page === 'addService' && <AddService />}
      {page === 'editService' && <EditService />}
    </div>
  );
}

export default App;
