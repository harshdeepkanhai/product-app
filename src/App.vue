<script setup lang="ts">
import { computed, ref } from 'vue'
import greenSocks from './assets/vmSocks-green.jpg'
import blueSocks from './assets/vmSocks-blue.jpg'

interface Variant {
  id: number
  color: string
  image: string
  quantity: number
}

const brand = 'Vue Mastery'
const product = 'Socks'
const onSale = true
const details = ['80% cotton', '20% polyester', 'Gender-neutral']

const variants: Variant[] = [
  { id: 2234, color: 'green', image: greenSocks, quantity: 10 },
  { id: 2235, color: 'blue', image: blueSocks, quantity: 0 },
]

const selectedVariant = ref(0)
const cart = ref(0)

const selected = computed(() => variants[selectedVariant.value]!)
const title = computed(() => `${brand} ${product}`)
const image = computed(() => selected.value.image)
const inStock = computed(() => selected.value.quantity > 0)
const sale = computed(() => (onSale ? `${title.value} is on sale!` : ''))

function addToCart() {
  cart.value++
}

function removeFromCart() {
  if (cart.value > 0) cart.value--
}

function updateVariant(index: number) {
  selectedVariant.value = index
}
</script>

<template>
  <header class="nav-bar">
    <span class="nav-brand">{{ brand }}</span>
    <div class="cart" :class="{ 'cart--filled': cart > 0 }">
      <span class="cart-icon">🛒</span>
      <span>{{ cart }}</span>
    </div>
  </header>

  <main class="product">
    <div class="product-image">
      <img :src="image" :alt="title" />
    </div>

    <div class="product-info">
      <span v-if="sale" class="badge">On Sale</span>
      <h1>{{ title }}</h1>

      <p class="stock" :class="inStock ? 'in' : 'out'">
        {{ inStock ? 'In Stock' : 'Out of Stock' }}
      </p>

      <ul class="details">
        <li v-for="detail in details" :key="detail">{{ detail }}</li>
      </ul>

      <div class="swatches">
        <button
          v-for="(variant, index) in variants"
          :key="variant.id"
          class="color-box"
          :class="{ active: index === selectedVariant }"
          :style="{ backgroundColor: variant.color }"
          :aria-label="variant.color"
          @mouseover="updateVariant(index)"
        ></button>
      </div>

      <div class="cart-actions">
        <button class="btn btn--primary" :disabled="!inStock" @click="addToCart">
          Add to cart
        </button>
        <button class="btn btn--ghost" :disabled="cart === 0" @click="removeFromCart">
          Remove
        </button>
      </div>
    </div>
  </main>
</template>

<style scoped>
/* ── Nav bar ─────────────────────────────── */
.nav-bar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 64px;
  padding: 0 32px;
  background: var(--gradient);
  color: #fff;
  box-shadow: var(--shadow);
}

.nav-brand {
  font-weight: 700;
  font-size: 1.15rem;
  letter-spacing: 0.3px;
}

.cart {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 8px 16px;
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.2);
  font-weight: 600;
  transition: background 0.2s ease;
}

.cart--filled {
  background: rgba(255, 255, 255, 0.38);
}

/* ── Product card ────────────────────────── */
.product {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 40px;
  align-items: center;
  max-width: 960px;
  margin: 40px auto;
  padding: 32px;
  background: #fff;
  border-radius: var(--radius);
  box-shadow: var(--shadow);
}

.product-image img {
  display: block;
  width: 100%;
  border-radius: 12px;
  border: 1px solid var(--line);
}

.product-info h1 {
  margin: 10px 0 12px;
  font-size: 2rem;
  font-weight: 700;
}

.badge {
  display: inline-block;
  padding: 4px 12px;
  border-radius: 999px;
  background: var(--gradient);
  color: #fff;
  font-size: 0.72rem;
  font-weight: 600;
  letter-spacing: 0.5px;
  text-transform: uppercase;
}

/* ── Stock indicator ─────────────────────── */
.stock {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  margin: 0 0 16px;
  font-weight: 500;
}

.stock::before {
  content: '';
  width: 9px;
  height: 9px;
  border-radius: 50%;
}

.stock.in {
  color: #0a7d4b;
}
.stock.in::before {
  background: #18b566;
}

.stock.out {
  color: #b42318;
}
.stock.out::before {
  background: #f04438;
}

/* ── Detail list ─────────────────────────── */
.details {
  list-style: none;
  padding: 0;
  margin: 0 0 24px;
  color: var(--muted);
}

.details li {
  position: relative;
  padding-left: 22px;
  margin-bottom: 6px;
}

.details li::before {
  content: '✓';
  position: absolute;
  left: 0;
  color: var(--accent-1);
  font-weight: 700;
}

/* ── Color swatches ──────────────────────── */
.swatches {
  display: flex;
  gap: 12px;
  margin-bottom: 28px;
}

.color-box {
  width: 42px;
  height: 42px;
  padding: 0;
  border: 2px solid #fff;
  border-radius: 50%;
  box-shadow: 0 0 0 1px var(--line);
  cursor: pointer;
  transition:
    transform 0.15s ease,
    box-shadow 0.15s ease;
}

.color-box:hover {
  transform: scale(1.1);
}

.color-box.active {
  box-shadow: 0 0 0 2px var(--accent-1);
}

/* ── Buttons ─────────────────────────────── */
.cart-actions {
  display: flex;
  gap: 12px;
}

.btn {
  height: 46px;
  padding: 0 22px;
  border: none;
  border-radius: 10px;
  font-family: inherit;
  font-size: 0.95rem;
  font-weight: 600;
  cursor: pointer;
  transition:
    transform 0.1s ease,
    box-shadow 0.2s ease,
    opacity 0.2s ease;
}

.btn--primary {
  background: var(--gradient);
  color: #fff;
  box-shadow: 0 6px 16px rgba(22, 192, 176, 0.35);
}

.btn--primary:hover:not(:disabled) {
  transform: translateY(-1px);
}

.btn--ghost {
  background: #fff;
  color: var(--ink);
  border: 1px solid var(--line);
}

.btn--ghost:hover:not(:disabled) {
  border-color: var(--muted);
}

.btn:disabled {
  opacity: 0.5;
  box-shadow: none;
  cursor: not-allowed;
}

/* ── Responsive ──────────────────────────── */
@media (max-width: 720px) {
  .product {
    grid-template-columns: 1fr;
    margin: 20px;
    padding: 20px;
  }
}
</style>
