/*
ملف: App.jsx
وصف: تطبيق React أحادي الملف لعرض منتجات وبيعها (قائمة، صفحة منتج، سلة شراء، نموذج "الدفع" تجريبي)
تعليمات سريعة (بالعربية):
1. أنشئ مشروع React (مثلاً: npx create-react-app my-shop)
2. ثبت Tailwind وفعّله أو استخدم أي CSS تفضل (الكود يستخدم فصول Tailwind).
3. استبدل محتوى src/App.jsx بالمحتوى التالي، ثم شغّل المشروع (npm start).
4. الدفع هنا محاكاة — إذا أردت ربط بوابة دفع فعلية (Stripe, PayPal) أخبرني لأدرج تعليمات.

ملاحظات تقنية:
- يستخدم التخزين المحلي localStorage لحفظ السلة بين زيارات المستخدم.
- جاهز للتطوير: اضافة صفحة تحكم للمنتجات، ربط API، إضافة لوحة إدارة.
*/

import React, { useState, useEffect } from 'react';

// بيانات عينات للمنتجات
const SAMPLE_PRODUCTS = [
  {
    id: 'p1',
    title: 'قميص قطني أنيق',
    price: 29.99,
    image: 'https://images.unsplash.com/photo-1520975910851-1b2f7f3b1d5c?auto=format&fit=crop&w=800&q=60',
    description: 'قميص مريح من القطن، مناسب لكل الأيام.'
  },
  {
    id: 'p2',
    title: 'حقيبة ظهر عملية',
    price: 49.5,
    image: 'https://images.unsplash.com/photo-1505740420928-5e560c06d30e?auto=format&fit=crop&w=800&q=60',
    description: 'حقيبة ظهر واسعة ومقاومة للماء مع عدة جيوب.'
  },
  {
    id: 'p3',
    title: 'سماعات لاسلكية',
    price: 79.0,
    image: 'https://images.unsplash.com/photo-1518444029371-0a9d1f0f0b78?auto=format&fit=crop&w=800&q=60',
    description: 'جودة صوت عالية وعمر بطارية طويل.'
  },
  {
    id: 'p4',
    title: 'سوار ذكي',
    price: 59.9,
    image: 'https://images.unsplash.com/photo-1526178613-9d3a0c8f0f9b?auto=format&fit=crop&w=800&q=60',
    description: 'تابع نشاطك اليومي وقياس النوم.'
  }
];

function useLocalStorage(key, initial) {
  const [state, setState] = useState(() => {
    try {
      const raw = window.localStorage.getItem(key);
      return raw ? JSON.parse(raw) : initial;
    } catch (e) {
      return initial;
    }
  });

  useEffect(() => {
    try {
      window.localStorage.setItem(key, JSON.stringify(state));
    } catch (e) {}
  }, [key, state]);

  return [state, setState];
}

export default function App() {
  const [products] = useState(SAMPLE_PRODUCTS);
  const [cart, setCart] = useLocalStorage('shop_cart_v1', {});
  const [query, setQuery] = useState('');
  const [selected, setSelected] = useState(null);
  const [showCart, setShowCart] = useState(false);
  const [checkoutStatus, setCheckoutStatus] = useState(null);

  function addToCart(productId, qty = 1) {
    setCart(prev => {
      const next = { ...prev };
      next[productId] = (next[productId] || 0) + qty;
      return next;
    });
    setShowCart(true);
  }

  function removeFromCart(productId) {
    setCart(prev => {
      const next = { ...prev };
      delete next[productId];
      return next;
    });
  }

  function updateQty(productId, qty) {
    if (qty <= 0) return removeFromCart(productId);
    setCart(prev => ({ ...prev, [productId]: qty }));
  }

  function clearCart() {
    setCart({});
  }

  function getCartItems() {
    return Object.entries(cart).map(([id, qty]) => {
      const p = products.find(x => x.id === id);
      return { ...p, qty };
    });
  }

  function cartTotal() {
    return getCartItems().reduce((s, it) => s + it.price * it.qty, 0);
  }

  function simulateCheckout(customer) {
    // محاكاة عملية الدفع: هنا نعيد نتيجة ناجحة بعد فحص بسيط
    if (!customer || !customer.name || !customer.phone) {
      setCheckoutStatus({ ok: false, message: 'املأ بيانات العميل (الاسم، رقم الهاتف).' });
      return;
    }
    if (Object.keys(cart).length === 0) {
      setCheckoutStatus({ ok: false, message: 'السلة فارغة.' });
      return;
    }

    // محاكاة نجاح
    setCheckoutStatus({ ok: true, message: `تمت عملية الشراء بنجاح — إجمالي: $${cartTotal().toFixed(2)}` });
    clearCart();
  }

  const filtered = products.filter(p => p.title.includes(query) || p.description.includes(query));

  return (
    <div className="min-h-screen bg-gray-50 text-gray-900">
      <header className="bg-white shadow sticky top-0 z-10">
        <div className="max-w-6xl mx-auto px-4 py-4 flex items-center justify-between">
          <h1 className="text-2xl font-semibold">متجري البسيط</h1>
          <div className="flex items-center gap-4">
            <input
              value={query}
              onChange={e => setQuery(e.target.value)}
              placeholder="ابحث عن منتج..."
              className="px-3 py-2 border rounded-md"
            />
            <button onClick={() => setShowCart(v => !v)} className="px-3 py-2 border rounded-md">
              سلة ( {Object.values(cart).reduce((a,b)=>a+b,0)} )
            </button>
          </div>
        </div>
      </header>

      <main className="max-w-6xl mx-auto px-4 py-8 grid grid-cols-1 lg:grid-cols-4 gap-8">
        <section className="lg:col-span-3">
          <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-6">
            {filtered.map(p => (
              <article key={p.id} className="bg-white rounded-lg shadow p-4 flex flex-col">
                <img src={p.image} alt={p.title} className="h-40 object-cover rounded-md mb-3" />
                <h2 className="font-semibold text-lg">{p.title}</h2>
                <p className="text-sm text-gray-600 flex-1 mt-2">{p.description}</p>
                <div className="mt-3 flex items-center justify-between">
                  <div className="text-lg font-bold">${p.price.toFixed(2)}</div>
                  <div className="flex gap-2">
                    <button onClick={() => { setSelected(p); }} className="px-3 py-1 border rounded">تصفح</button>
                    <button onClick={() => addToCart(p.id)} className="px-3 py-1 bg-blue-600 text-white rounded">أضف إلى السلة</button>
                  </div>
                </div>
              </article>
            ))}
          </div>
        </section>

        <aside className="lg:col-span-1">
          <div className="bg-white rounded-lg shadow p-4">
            <h3 className="font-semibold mb-2">نظرة عامة</h3>
            <p>منتجات متاحة: <strong>{products.length}</strong></p>
            <p>إجمالي السلة: <strong>${cartTotal().toFixed(2)}</strong></p>
            <div className="mt-4 flex flex-col gap-2">
              <button onClick={() => setShowCart(true)} className="px-3 py-2 bg-green-600 text-white rounded">الذهاب للسلة</button>
              <button onClick={() => { setSelected(null); setQuery(''); }} className="px-3 py-2 border rounded">إعادة تعيين</button>
            </div>
          </div>

          <div className="mt-4 bg-white rounded-lg shadow p-4">
            <h4 className="font-semibold mb-2">نصائح</h4>
            <ul className="text-sm text-gray-600 list-disc list-inside">
              <li>ارتباط الدفع هنا محاكاة—استخدم Stripe أو PayPal للرابط الحي.</li>
              <li>لإضافة منتجات، قم بتعديل مصفوفة SAMPLE_PRODUCTS أو ربط API.</li>
            </ul>
          </div>
        </aside>
      </main>

      {/* سلة عائمة */}
      {showCart && (
        <div className="fixed right-4 bottom-4 w-96 bg-white shadow-lg rounded-lg p-4">
          <div className="flex items-center justify-between mb-3">
            <h3 className="text-lg font-semibold">سلة التسوق</h3>
            <button onClick={() => setShowCart(false)} className="text-sm">إغلاق</button>
          </div>

          {getCartItems().length === 0 ? (
            <div className="text-gray-600">السلة فارغة.</div>
          ) : (
            <div className="space-y-3 max-h-64 overflow-auto">
              {getCartItems().map(it => (
                <div key={it.id} className="flex items-center gap-3">
                  <img src={it.image} alt={it.title} className="w-16 h-16 object-cover rounded" />
                  <div className="flex-1">
                    <div className="font-medium">{it.title}</div>
                    <div className="text-sm text-gray-600">${it.price.toFixed(2)} × {it.qty} = ${(it.price * it.qty).toFixed(2)}</div>
                    <div className="mt-2 flex items-center gap-2">
                      <button onClick={() => updateQty(it.id, it.qty - 1)} className="px-2 py-1 border rounded">-</button>
                      <input value={it.qty} onChange={e=>{ const v = parseInt(e.target.value||0,10); if(!isNaN(v)) updateQty(it.id, v); }} className="w-12 text-center border rounded" />
                      <button onClick={() => updateQty(it.id, it.qty + 1)} className="px-2 py-1 border rounded">+</button>
                      <button onClick={() => removeFromCart(it.id)} className="ml-auto text-sm text-red-600">إزالة</button>
                    </div>
                  </div>
                </div>
              ))}

              <div className="border-t pt-3">
                <div className="flex justify-between font-semibold">المجموع: <span>${cartTotal().toFixed(2)}</span></div>
                <CheckoutForm onCheckout={simulateCheckout} />
              </div>
            </div>
          )}
        </div>
      )}

      {selected && (
        <div className="fixed inset-0 bg-black/40 flex items-center justify-center p-4">
          <div className="bg-white rounded-lg max-w-2xl w-full p-6">
            <div className="flex gap-4">
              <img src={selected.image} alt={selected.title} className="w-48 h-48 object-cover rounded" />
              <div className="flex-1">
                <h2 className="text-xl font-semibold">{selected.title}</h2>
                <p className="text-gray-600 mt-2">{selected.description}</p>
                <div className="mt-4 text-2xl font-bold">${selected.price.toFixed(2)}</div>
                <div className="mt-4 flex gap-2">
                  <button onClick={() => addToCart(selected.id)} className="px-4 py-2 bg-blue-600 text-white rounded">أضف للسلة</button>
                  <button onClick={() => setSelected(null)} className="px-4 py-2 border rounded">إغلاق</button>
                </div>
              </div>
            </div>
          </div>
        </div>
      )}

      {checkoutStatus && (
        <div className={`fixed left-4 bottom-4 p-3 rounded ${checkoutStatus.ok ? 'bg-green-100 border border-green-300' : 'bg-red-100 border border-red-300'}`}>
          <div className="font-medium">{checkoutStatus.ok ? 'نجاح' : 'خطأ'}</div>
          <div className="text-sm">{checkoutStatus.message}</div>
          <div className="text-right mt-2">
            <button onClick={() => setCheckoutStatus(null)} className="text-sm underline">حسناً</button>
          </div>
        </div>
      )}

    </div>
  );
}

function CheckoutForm({ onCheckout }) {
  const [name, setName] = useState('');
  const [phone, setPhone] = useState('');
  const [address, setAddress] = useState('');

  return (
    <div className="mt-3">
      <h4 className="font-semibold mb-2">بيانات المشتري</h4>
      <input value={name} onChange={e=>setName(e.target.value)} placeholder="الاسم" className="w-full mb-2 px-2 py-1 border rounded" />
      <input value={phone} onChange={e=>setPhone(e.target.value)} placeholder="رقم الهاتف" className="w-full mb-2 px-2 py-1 border rounded" />
      <input value={address} onChange={e=>setAddress(e.target.value)} placeholder="العنوان (اختياري)" className="w-full mb-2 px-2 py-1 border rounded" />
      <div className="flex gap-2">
        <button onClick={() => onCheckout({ name, phone, address })} className="flex-1 px-3 py-2 bg-blue-600 text-white rounded">إتمام الشراء (محاكاة)</button>
        <button onClick={() => { setName(''); setPhone(''); setAddress(''); }} className="px-3 py-2 border rounded">مسح</button>
      </div>
    </div>
  );
}
