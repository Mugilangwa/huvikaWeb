<template>
    <!-- Tumeongeza position-relative na overflow-hidden kwa ajili ya mapambo -->
    <footer class="site-footer position-relative overflow-hidden">
        
        <!-- Mstari wa juu wenye rangi zetu za kanisa (Crimson na Gold) -->
        <div class="position-absolute top-0 start-0 w-100" style="height: 4px; background: linear-gradient(90deg, #7f1d1d 0%, #d4a017 50%, #7f1d1d 100%);"></div>

        <!-- Mapambo hafifu ya background (Inafanya isiwe nyeusi tupu) -->
        <div class="position-absolute top-0 start-0 w-100 h-100 opacity-25" style="background-image: radial-gradient(circle at 10% 90%, rgba(212,160,23,0.15) 0%, transparent 50%); pointer-events: none;"></div>

        <div class="container position-relative z-2">

            <!-- ============================================================
                 MAIN ROW
            ============================================================ -->
            <div class="row g-4 g-lg-5 pb-4">

                <!-- Brand -->
                <div class="col-lg-4 col-md-6">
                    <h3 class="footer-brand-title">HUVIKA</h3>
                    <p class="footer-tagline">{{ $t('footer.tagline') }}</p>
                    <p class="footer-desc">{{ $t('footer.description') }}</p>
                    <div class="d-flex gap-2">
                        <a href="#" class="footer-social-btn" aria-label="Facebook">
                            <i class="bi bi-facebook"></i>
                        </a>
                        <a href="#" class="footer-social-btn" aria-label="Instagram">
                            <i class="bi bi-instagram"></i>
                        </a>
                        <a href="#" class="footer-social-btn" aria-label="Twitter">
                            <i class="bi bi-twitter-x"></i>
                        </a>
                        <a href="#" class="footer-social-btn" aria-label="YouTube">
                            <i class="bi bi-youtube"></i>
                        </a>
                    </div>
                </div>

                <!-- Links -->
                <div class="col-6 col-md-3 col-lg-2">
                    <h5 class="footer-heading">{{ $t('footer.links') }}</h5>
                    <ul class="list-unstyled footer-links">
                        <li>
                            <router-link to="/">
                                <i class="bi bi-chevron-right me-1"></i> {{ $t('nav.home') }}
                            </router-link>
                        </li>
                        <li>
                            <router-link to="/about">
                                <i class="bi bi-chevron-right me-1"></i> {{ $t('nav.about') }}
                            </router-link>
                        </li>
                        <li>
                            <router-link to="/parish">
                                <i class="bi bi-chevron-right me-1"></i> {{ $t('nav.parish') }}
                            </router-link>
                        </li>
                        <li>
                            <router-link to="/services">
                                <i class="bi bi-chevron-right me-1"></i> {{ $t('nav.programs') }}
                            </router-link>
                        </li>
                        <li>
                            <router-link to="/contact">
                                <i class="bi bi-chevron-right me-1"></i> {{ $t('nav.contact') }}
                            </router-link>
                        </li>
                    </ul>
                </div>

                <!-- Services -->
                <div class="col-6 col-md-3 col-lg-2">
                    <h5 class="footer-heading">{{ $t('footer.services') }}</h5>
                    <ul class="list-unstyled footer-links">
                        <li>
                            <router-link to="/services">
                                <i class="bi bi-chevron-right me-1"></i> {{ $t('programs.items.spiritual') }}
                            </router-link>
                        </li>
                        <li>
                            <router-link to="/services">
                                <i class="bi bi-chevron-right me-1"></i> {{ $t('programs.items.leadership') }}
                            </router-link>
                        </li>
                        <li>
                            <router-link to="/services">
                                <i class="bi bi-chevron-right me-1"></i> {{ $t('programs.items.community') }}
                            </router-link>
                        </li>
                        <li>
                            <router-link to="/services">
                                <i class="bi bi-chevron-right me-1"></i> {{ $t('programs.items.sports') }}
                            </router-link>
                        </li>
                    </ul>
                </div>

                <!-- Newsletter -->
                <div class="col-lg-4 col-md-6">
                    <h5 class="footer-heading">{{ $t('footer.newsletter') }}</h5>
                    <p class="footer-desc mb-3">{{ $t('footer.newsletterText') }}</p>
                    <form class="footer-newsletter" @submit.prevent="subscribe">
                        <div class="input-group shadow-sm rounded-3">
                            <input
                                type="email"
                                class="form-control"
                                v-model="email"
                                :placeholder="$t('footer.emailPlaceholder')"
                                required
                            />
                            <button class="btn btn-gold" type="submit" :disabled="loading">
                                <span v-if="loading" class="spinner-border spinner-border-sm me-1" role="status"></span>
                                <i v-else class="bi bi-send-fill me-1"></i>
                                {{ loading ? '...' : $t('footer.joinBtn') }}
                            </button>
                        </div>
                    </form>
                </div>
            </div>

            <!-- ============================================================
                 BOTTOM BAR
            ============================================================ -->
            <div class="footer-bottom d-flex flex-column flex-md-row justify-content-between align-items-center gap-2">
                <span class="small">
                    &copy; {{ currentYear }}
                    <router-link to="/" class="footer-bottom-link fw-bold">HUVIKA {{ $t('footer.tagline') }}</router-link>.
                    {{ $t('footer.copyright') }}
                </span>
                <span class="small d-flex align-items-center gap-1">
                    {{ $t('footer.builtBy') }}
                    <a href="#" class="footer-bottom-link fw-bold">HUVIKA</a>
                    <i class="bi bi-heart-fill text-danger ms-1" style="font-size: 10px;"></i>
                </span>
            </div>
        </div>
    </footer>
</template>

<script setup>
import { ref, computed } from 'vue';
import axios from 'axios';
import Swal from 'sweetalert2';

const email = ref('');
const loading = ref(false);
const currentYear = computed(() => new Date().getFullYear());

async function subscribe() {
    if (!email.value) return;
    loading.value = true;
    try {
        await axios.post('/api/subscribe/', { email: email.value });
        Swal.fire({
            icon: 'success',
            title: 'Asante!',
            text: 'Umejiunga na jarida letu.',
            confirmButtonColor: '#b91c1c',
        });
        email.value = '';
    } catch (err) {
        Swal.fire({
            icon: 'error',
            title: 'Hitilafu',
            text: 'Imeshindwa. Jaribu tena baadaye.',
            confirmButtonColor: '#b91c1c',
        });
    } finally {
        loading.value = false;
    }
}
</script>

<style scoped>
/* ============================================================
   FOOTER WRAPPER
============================================================ */
.site-footer {
    /* Tumia gradient nzuri ya rangi nyeusi na nyekundu iliyokolea */
    background: linear-gradient(180deg, #111111 0%, #1a0a0a 100%);
    color: rgba(255, 255, 255, 0.7);
    padding: 64px 0 0;
    border-top: 1px solid rgba(255, 255, 255, 0.05);
}

/* ============================================================
   BRAND
============================================================ */
.footer-brand-title {
    font-family: var(--fh);
    font-size: 32px;
    font-weight: 900;
    color: #fff;
    margin-bottom: 6px;
    letter-spacing: 2px;
    /* Inafanya jina lionekane kama linang'aa */
    text-shadow: 0 2px 10px rgba(212, 160, 23, 0.3);
}
.footer-tagline {
    font-family: var(--fa);
    font-style: italic;
    font-size: 15px;
    color: #d4a017; /* Dhahabu */
    margin-bottom: 16px;
    letter-spacing: 1px;
}
.footer-desc {
    font-size: 13.5px;
    line-height: 1.8;
    margin-bottom: 20px;
    color: rgba(255, 255, 255, 0.65);
}

/* ============================================================
   SOCIAL BUTTONS
============================================================ */
.footer-social-btn {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    width: 38px;
    height: 38px;
    border-radius: 50%; /* Zimekuwa mviringo */
    background: rgba(255, 255, 255, 0.05);
    border: 1px solid rgba(255, 255, 255, 0.1);
    color: rgba(255, 255, 255, 0.75);
    font-size: 15px;
    transition: all 0.3s ease;
    text-decoration: none;
}
.footer-social-btn:hover {
    background: #7f1d1d; /* Crimson */
    border-color: #7f1d1d;
    color: #fff;
    transform: translateY(-3px); /* Inapanda juu */
    box-shadow: 0 5px 15px rgba(127, 29, 29, 0.4);
}

/* ============================================================
   HEADINGS + LINKS
============================================================ */
.footer-heading {
    font-family: var(--fh);
    font-size: 16px;
    font-weight: 700;
    color: #fff;
    margin-bottom: 20px;
    padding-bottom: 10px;
    border-bottom: 2px solid rgba(212, 160, 23, 0.3); /* Dhahabu hafifu */
}
.footer-links {
    padding: 0;
    margin: 0;
}
.footer-links li {
    margin-bottom: 12px;
}
.footer-links a {
    font-size: 13.5px;
    color: rgba(255, 255, 255, 0.6);
    text-decoration: none;
    display: inline-flex;
    align-items: center;
    transition: all 0.2s ease;
}
.footer-links a:hover {
    color: #d4a017; /* Dhahabu */
    padding-left: 6px; /* Inasogea mbele ikiguswa */
}

/* ============================================================
   NEWSLETTER
============================================================ */
.footer-newsletter .form-control {
    background: rgba(255, 255, 255, 0.05);
    border: 1px solid rgba(255, 255, 255, 0.1);
    color: #fff;
    font-size: 13.5px;
    padding: 11px 16px;
    border-radius: 8px 0 0 8px;
}
.footer-newsletter .form-control:focus {
    background: rgba(255, 255, 255, 0.1);
    border-color: #d4a017;
    color: #fff;
    box-shadow: none;
}
.footer-newsletter .form-control::placeholder {
    color: rgba(255, 255, 255, 0.35);
}
.btn-gold {
    background: #d4a017; /* Dhahabu */
    color: #111;
    border: none;
    font-weight: 700;
    font-size: 13.5px;
    padding: 11px 20px;
    border-radius: 0 8px 8px 0;
    transition: all 0.2s ease;
}
.btn-gold:hover {
    background: #b8860b; /* Dhahabu iliyokolea */
    color: #111;
    transform: scale(1.02);
}
.btn-gold:disabled {
    opacity: 0.7;
}

/* ============================================================
   BOTTOM BAR
============================================================ */
.footer-bottom {
    padding: 24px 0;
    border-top: 1px solid rgba(255, 255, 255, 0.08);
    color: rgba(255, 255, 255, 0.4);
}
.footer-bottom-link {
    color: rgba(255, 255, 255, 0.6);
    text-decoration: none;
    transition: color 0.2s;
}
.footer-bottom-link:hover {
    color: #d4a017;
}
</style>