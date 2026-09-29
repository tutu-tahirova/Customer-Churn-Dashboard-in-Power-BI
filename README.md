# Customer-Churn-Dashboard-in-Power-BI

## 1. Power Query Çevirmələri və Qərarlar
* **Məlumat Tipləri:** Bütün sütunlara müvafiq məlumat tipləri təyin edildi.
* tenure = 0 olan yeni müştərilərə uyğun gələn boş sətirlərə görə bu sütun əvvəlcə mətn kimi import olunmuşdu. Həmin boşluqlar 0 ilə əvəz edildi və onluq rəqəm tipinə çevrildi ki, bu da yeni abunəçilərin hələlik heç bir ödəniş yığmadığını əks etdirir.
* * TenureBand: Müştərinin xidmətdə qalma müddəti qruplara bölündü (0-12, 13-24, 25-48, 49+ ay).
  * ChargeBand: MonthlyCharges üzrə müəyyən aralıqlarla göstərildi.
  * ChurnFlag: Müştəri tərk etməsinin rəqəmsal formata keçirilməsi (Yes -> 1, No -> 0).

## 3. Əsas DAX Ölçüləri
* Total Customers = COUNTROWS('WA_Fn-UseC_-Telco-Customer-Churn')
* Churned Customers = CALCULATE([Total Customers], 'WA_Fn-UseC_-Telco-Customer-Churn'[Churn] = "Yes")
* Churn Rate % = DIVIDE([Churned Customers], [Total Customers]) (Sıfıra bölünmə xətalarının qarşısını almaq üçün / operatoru əvəzinə DIVIDE funksiyasından istifadə edildi, bu zaman xəta yerinə boş dəyər qaytarılır).
* Avg Monthly Charges = AVERAGE('WA_Fn-UseC_-Telco-Customer-Churn'[MonthlyCharges])
* Monthly Revenue at Risk = CALCULATE(SUM('WA_Fn-UseC_-Telco-Customer-Churn'[MonthlyCharges]), 'WA_Fn-UseC_-Telco-Customer-Churn'[Churn] = "Yes")

## 4. Retention Tövsiyələri 
1. **İlk İl və Fiber Optik Müştəriləri Üçün Məqsədyönlü Onboarding :**
   * *İnsight:* Dashboard göstərir ki, müştəri itkisi (churn) əsasən ilk 12 ayda, xüsusilə də Fiber optik internet abunəçilərində yüksəkdir (~41.9% churn).
   * *Tədbir:* Texniki problemləri və narazılıqları erkən mərhələdə aradan qaldırmaq üçün xüsusilə yeni Fiber optik istifadəçiləri üçün 90 günlük xüsusi onbording və müştəri məmnuniyyəti proqramı tətbiq edilməlidir.
2. **Aylıq Müqaviləli İstifadəçiləri Uzunmüddətli Müqavilələrə Həvəsləndirmək :**
   * *İnsight:* Aylıq (Month-to-month) müqavilələr One-year (~11.3%) və Two-year (~2.8%) müqavilələri ilə müqayisədə hədsiz dərəcədə yüksək churn faizinə (~42.7%) malikdir və itirilən gəlirin əsas hissəsini təşkil edir.
   * *Tədbir:* Aylıq müştəriləri illik müqavilələrə keçməyə təşviq etmək üçün hədəflənmiş kampaniyalar, xüsusi endirimlər və rahat keçid təqdim olunmalıdır.
3. **Ödəniş Üsullarını Elektron Çeklərdən Avtomatik Ödənişə Yönləndirmək :**
   * *İnsight:* Elektron çek istifadə edənlər digər ödəniş kanalları ilə müqayisədə ən yüksək churn nisbətinə malikdirlər (~45.3%).
   * *Tədbir:* Əl ilə ödəniş edilən elektron çeklərdən avtomatik ödəniş sisteminə keçən müştərilərə kiçik birtəfəlik endirimlər və ya üstünlüklər təklif olunsun.
