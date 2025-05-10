---
layout: null
permalink: /
---

<script>
  const userLang = navigator.language || navigator.userLanguage;
  if (userLang.startsWith("zh")) {
    location.href = "{{ '/zh/' | relative_url }}";
  } else {
    location.href = "{{ '/en/' | relative_url }}";
  }
</script>
