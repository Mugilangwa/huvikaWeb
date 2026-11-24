<template>
    <div class="layout-px-spacing church-form-page m-4">
        <teleport to="#breadcrumb">
            <ul class="navbar-nav flex-row">
                <li>
                    <div class="page-header">
                        <nav class="breadcrumb-one" aria-label="breadcrumb">
                            <ol class="breadcrumb">
                                <li class="breadcrumb-item"><a href="javascript:;">Churches</a></li>
                                <li class="breadcrumb-item active"><span>{{ isEdit ? 'Edit Church' : 'Add Church' }}</span></li>
                            </ol>
                        </nav>
                    </div>
                </li>
            </ul>
        </teleport>

        <!-- Hero Header -->
        <div class="hero-header mb-4">
            <div class="hero-content">
                <div class="hero-text">
                    <div class="hero-icon">
                        <svg xmlns="http://www.w3.org/2000/svg" width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
                            <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"></path>
                            <polyline points="22,6 12,13 2,6"></polyline>
                        </svg>
                    </div>
                    <div>
                        <h1 class="hero-title">{{ isEdit ? 'Edit Church' : 'Church Registration' }}</h1>
                        <p class="hero-subtitle">{{ isEdit ? 'Update church information and details' : 'Register a new church in the system' }}</p>
                    </div>
                </div>
                <router-link to="/parishi" class="btn-modern btn-ghost">
                    <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                        <line x1="19" y1="12" x2="5" y2="12"></line>
                        <polyline points="12 19 5 12 12 5"></polyline>
                    </svg>
                    <span>Back to List</span>
                </router-link>
            </div>
        </div>

        <form @submit.prevent="saveChurch">
            <div class="row">
                <!-- Left Column -->
                <div class="col-xl-8">
                    <div class="modern-panel mb-4">
                        <div class="panel-header">
                            <h5>Basic Information</h5>
                            <p>Core details about the church</p>
                        </div>
                        <div class="panel-body">
                            <div class="row">
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label>Church Name <span class="req">*</span></label>
                                        <input type="text" v-model="form.name" class="form-control" placeholder="Enter church name" required>
                                    </div>
                                </div>
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label>Church Code <span class="req">*</span></label>
                                        <input type="text" v-model="form.code" class="form-control" placeholder="e.g., CCO-001" required>
                                    </div>
                                </div>
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label>Also Known As (AKA)</label>
                                        <input type="text" v-model="form.aka" class="form-control" placeholder="Nickname">
                                    </div>
                                </div>
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label>Phone Number</label>
                                        <input type="text" v-model="form.phone" class="form-control" placeholder="Phone number">
                                    </div>
                                </div>
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label>Status</label>
                                        <select v-model="form.is_active" class="form-control">
                                            <option :value="true">Active</option>
                                            <option :value="false">Inactive</option>
                                        </select>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div class="modern-panel mb-4">
                        <div class="panel-header">
                            <h5>Location Information</h5>
                            <p>Address and administrative location</p>
                        </div>
                        <div class="panel-body">
                            <div class="row">
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label>Administrative Location</label>
                                        <select v-model="form.location" class="form-control">
                                            <option :value="null">Select Location</option>
                                            <option v-for="loc in locations" :key="loc.id" :value="loc.id">
                                                {{ loc.full_path || loc.name }}
                                            </option>
                                        </select>
                                    </div>
                                </div>
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label>Address</label>
                                        <input v-model="form.address" class="form-control" placeholder="Full address"/>
                                    </div>
                                </div>
                                <div class="col-12">
                                    <div class="form-group">
                                        <label>Description</label>
                                        <textarea v-model="form.description" class="form-control" rows="3" placeholder="Church description"></textarea>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div class="modern-panel mb-4">
                        <div class="panel-header">
                            <h5>Map Location</h5>
                            <p>Geographic coordinates for the church</p>
                        </div>
                        <div class="panel-body">
                            <div class="row">
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label>Latitude</label>
                                        <input type="text" v-model="geolocation.coordinates.lat" class="form-control" placeholder="-6.8234">
                                    </div>
                                </div>
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label>Longitude</label>
                                        <input type="text" v-model="geolocation.coordinates.lng" class="form-control" placeholder="39.2345">
                                    </div>
                                </div>
                                <div class="col-12">
                                    <div class="btn-row">
                                        <button type="button" class="btn-modern btn-success-modern" @click="getCurrentLocation">
                                            <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                                <circle cx="12" cy="12" r="10"></circle>
                                                <polygon points="12 2 15 9 22 9 16 14 19 22 12 17 5 22 8 14 2 9 9 9 12 2"></polygon>
                                            </svg>
                                            <span>Use My Current Location</span>
                                        </button>
                                        <button type="button" class="btn-modern btn-info-modern" @click="openGoogleMaps">
                                            <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                                <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"></path>
                                                <circle cx="12" cy="10" r="3"></circle>
                                            </svg>
                                            <span>Open Google Maps</span>
                                        </button>
                                    </div>
                                    <small class="text-muted d-block mt-2">Click "Open Google Maps" to find your location, then copy coordinates above</small>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Right Column -->
                <div class="col-xl-4">
                    <div class="modern-panel mb-4">
                        <div class="panel-header">
                            <h5>Church Profile Picture</h5>
                            <p>Upload a logo or profile image</p>
                        </div>
                        <div class="panel-body text-center">
                            <div class="profile-upload-container mb-3">
                                <div class="profile-preview" @click="triggerFileInput">
                                    <img v-if="profilePreview" :src="profilePreview" alt="Church Profile" class="profile-image" />
                                    <div v-else class="profile-placeholder">
                                        <svg width="60" height="60" viewBox="0 0 24 24" fill="none" stroke="#cbd5e0" stroke-width="1">
                                            <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"></path>
                                            <polyline points="22,6 12,13 2,6"></polyline>
                                            <circle cx="12" cy="13" r="3"></circle>
                                        </svg>
                                        <p class="mt-2 text-muted">Click to upload</p>
                                    </div>
                                </div>
                                <input type="file" ref="fileInput" @change="handleImageUpload" accept="image/*" style="display: none" />
                            </div>
                            <div class="btn-row justify-content-center">
                                <button type="button" class="btn-modern btn-outline-modern" @click="triggerFileInput">
                                    <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                        <polyline points="16 16 12 12 8 16"></polyline>
                                        <line x1="12" y1="12" x2="12" y2="21"></line>
                                        <path d="M20.39 18.39A5 5 0 0 0 18 9h-1.26A8 8 0 1 0 3 16.3"></path>
                                    </svg>
                                    <span>Upload</span>
                                </button>
                                <button type="button" class="btn-modern btn-danger-modern" @click="removeImage" v-if="profilePreview">
                                    <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                        <polyline points="3 6 5 6 21 6"></polyline>
                                        <path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"></path>
                                    </svg>
                                    <span>Remove</span>
                                </button>
                            </div>
                            <small class="text-muted d-block mt-2">Recommended: Square image, max 2MB</small>
                        </div>
                    </div>

                    <div class="modern-panel">
                        <div class="panel-body">
                            <button type="submit" class="btn-modern btn-primary-modern w-100 mb-2" :disabled="loading">
                                <svg v-if="loading" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="spin">
                                    <circle cx="12" cy="12" r="10"></circle>
                                    <line x1="12" y1="2" x2="12" y2="6"></line>
                                    <line x1="12" y1="18" x2="12" y2="22"></line>
                                </svg>
                                <svg v-else width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                    <path d="M19 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11l5 5v11a2 2 0 0 1-2 2z"></path>
                                    <polyline points="17 21 17 13 7 13 7 21"></polyline>
                                    <polyline points="7 3 7 8 15 8"></polyline>
                                </svg>
                                <span>{{ loading ? 'Saving...' : (isEdit ? 'Update Church' : 'Save Church') }}</span>
                            </button>
                            <router-link to="/parishi" class="btn-modern btn-ghost w-100">
                                <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                    <line x1="18" y1="6" x2="6" y2="18"></line>
                                    <line x1="6" y1="6" x2="18" y2="18"></line>
                                </svg>
                                <span>Cancel</span>
                            </router-link>
                        </div>
                    </div>
                </div>
            </div>
        </form>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useRoute, useRouter } from 'vue-router';
import { axiosInstance } from '@/services/axios';
import { useMeta } from '@/composables/use-meta';
import { useNotification } from '@/composables/swal';

const { showAlert } = useNotification();
useMeta({ title: 'Church Form' });

const route = useRoute();
const router = useRouter();
const isEdit = ref(false);
const loading = ref(false);
const locations = ref([]);
const fileInput = ref(null);
const profilePreview = ref(null);
const profileImageFile = ref(null);

const geolocation = ref({
    id: null,
    name: '',
    geometry_type: 'point',
    coordinates: { lat: -6.8234, lng: 39.2345 },
    metadata: {}
});

const form = ref({
    name: '',
    code: '',
    aka: '',
    phone: '',
    location: null,
    address: '',
    description: '',
    is_active: true,
    geolocation: null,
    profile_image: null
});

const loadLocations = async () => {
    try {
        const response = await axiosInstance.get('/locations/');
        locations.value = response.data;
    } catch (error) {
        console.error('Error loading locations:', error);
    }
};

const loadChurch = async (id) => {
    try {
        const response = await axiosInstance.get(`/churches/${id}/`);
        const data = response.data;
        form.value = {
            name: data.name,
            code: data.code,
            aka: data.aka || '',
            phone: data.phone || '',
            location: data.location,
            address: data.address || '',
            description: data.description || '',
            is_active: data.is_active,
            geolocation: data.geolocation,
            profile_image: data.profile_image
        };
        if (data.profile_image) profilePreview.value = data.profile_image;
        if (data.geolocation && data.geolocation.coordinates) {
            const coords = data.geolocation.coordinates;
            geolocation.value = {
                id: data.geolocation.id,
                name: `${data.name} Location`,
                geometry_type: 'point',
                coordinates: { lat: coords.coordinates[1], lng: coords.coordinates[0] },
                metadata: data.geolocation.metadata || {}
            };
        }
    } catch (error) {
        console.error('Error loading church:', error);
    }
};

const triggerFileInput = () => fileInput.value.click();

const handleImageUpload = (event) => {
    const file = event.target.files[0];
    if (!file) return;
    if (file.size > 2 * 1024 * 1024) {
        showAlert('info', 'Image size should be less than 2MB');
        return;
    }
    if (!file.type.startsWith('image/')) {
        showAlert('info', 'Please upload an image file');
        return;
    }
    profileImageFile.value = file;
    const reader = new FileReader();
    reader.onload = (e) => { profilePreview.value = e.target.result; };
    reader.readAsDataURL(file);
};

const removeImage = () => {
    profilePreview.value = null;
    profileImageFile.value = null;
    form.value.profile_image = null;
    if (fileInput.value) fileInput.value.value = '';
};

const getCurrentLocation = () => {
    if (!navigator.geolocation) {
        showAlert('error', 'Geolocation is not supported by this browser.');
        return;
    }
    navigator.geolocation.getCurrentPosition(
        (position) => {
            const { latitude, longitude } = position.coords;
            geolocation.value.coordinates.lat = latitude;
            geolocation.value.coordinates.lng = longitude;
            showAlert('success', `Location updated! Lat: ${latitude}, Lng: ${longitude}`);
        },
        (error) => {
            console.error('Error getting location:', error);
            showAlert('error', 'Unable to get your location. Please check browser permissions.');
        }
    );
};

const openGoogleMaps = () => {
    const { lat, lng } = geolocation.value.coordinates;
    window.open(`https://www.google.com/maps?q=${lat},${lng}&z=15`, '_blank');
};

const uploadImage = async () => {
    if (!profileImageFile.value) return null;
    const formData = new FormData();
    formData.append('image', profileImageFile.value);
    formData.append('church_name', form.value.name);
    try {
        const response = await axiosInstance.post('/upload-church-image/', formData, {
            headers: { 'Content-Type': 'multipart/form-data' }
        });
        return response.data.image_url;
    } catch (error) {
        console.error('Error uploading image:', error);
        return null;
    }
};

const saveChurch = async () => {
    if (!form.value.name) { showAlert('error', 'Church name is required'); return; }
    if (!form.value.code) { showAlert('error', 'Church code is required'); return; }
    loading.value = true;
    try {
        let imageUrl = form.value.profile_image;
        if (profileImageFile.value) {
            const uploadedUrl = await uploadImage();
            if (uploadedUrl) imageUrl = uploadedUrl;
        }
        let geolocationId = geolocation.value.id;
        if (geolocation.value.coordinates.lat && geolocation.value.coordinates.lng) {
            const geoData = {
                name: `${form.value.name} Location`,
                geometry_type: 'point',
                coordinates: {
                    type: 'Point',
                    coordinates: [geolocation.value.coordinates.lng, geolocation.value.coordinates.lat]
                },
                metadata: { church_name: form.value.name, address: form.value.address },
                is_active: true
            };
            if (!geolocation.value.id) {
                const geoResponse = await axiosInstance.post('/geolocations/', geoData);
                geolocationId = geoResponse.data.id;
            } else {
                await axiosInstance.put(`/geolocations/${geolocation.value.id}/`, geoData);
            }
        }
        const churchData = { ...form.value, geolocation: geolocationId || null, profile_image: imageUrl };
        if (isEdit.value) {
            await axiosInstance.put(`/churches/${route.params.id}/`, churchData);
        } else {
            await axiosInstance.post('/churches/', churchData);
        }
        router.push('/parishi');
    } catch (error) {
        console.error('Error saving church:', error);
        showAlert('error', error.response?.data?.message || 'Error saving church');
    } finally {
        loading.value = false;
    }
};

onMounted(async () => {
    await loadLocations();
    if (route.params.id) {
        isEdit.value = true;
        await loadChurch(route.params.id);
    }
});
</script>

<style scoped>
.church-form-page {
    --primary: #4361ee;
    --primary-dark: #3a0ca3;
    --success: #10b981;
    --info: #0ea5e9;
    --danger: #ef4444;
    --gray-50: #f9fafb;
    --gray-100: #f3f4f6;
    --gray-200: #e5e7eb;
    --gray-500: #6b7280;
    --gray-700: #374151;
    --gray-900: #111827;
    --radius: 16px;
    --shadow-sm: 0 1px 3px rgba(0,0,0,0.06);
    --shadow-md: 0 4px 16px rgba(0,0,0,0.08);
    --shadow-lg: 0 12px 32px rgba(0,0,0,0.12);
}

/* Hero */
.hero-header {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    border-radius: var(--radius);
    padding: 28px 32px;
    position: relative;
    overflow: hidden;
    box-shadow: var(--shadow-lg);
}
.hero-header::before {
    content: '';
    position: absolute;
    top: -50%; right: -10%;
    width: 380px; height: 380px;
    background: rgba(255,255,255,0.08);
    border-radius: 50%;
}
.hero-content {
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
    gap: 20px;
    position: relative;
    z-index: 1;
}
.hero-text { display: flex; align-items: center; gap: 18px; }
.hero-icon {
    width: 56px; height: 56px;
    background: rgba(255,255,255,0.15);
    backdrop-filter: blur(10px);
    border-radius: 14px;
    display: flex; align-items: center; justify-content: center;
    color: #fff;
    border: 1px solid rgba(255,255,255,0.2);
}
.hero-title { font-size: 24px; font-weight: 700; color: #fff; margin: 0 0 4px 0; letter-spacing: -0.3px; }
.hero-subtitle { color: rgba(255,255,255,0.85); margin: 0; font-size: 13px; }

/* Panel */
.modern-panel {
    background: #fff;
    border-radius: var(--radius);
    box-shadow: var(--shadow-md);
    overflow: hidden;
    border: 1px solid var(--gray-200);
}
.panel-header {
    padding: 20px 24px;
    border-bottom: 1px solid var(--gray-100);
    background: var(--gray-50);
}
.panel-header h5 { margin: 0 0 2px 0; font-size: 16px; font-weight: 700; color: var(--gray-900); }
.panel-header p { margin: 0; font-size: 12px; color: var(--gray-500); }
.panel-body { padding: 24px; }

/* Form */
.form-group { margin-bottom: 18px; }
.form-group label {
    display: block;
    font-size: 13px;
    font-weight: 600;
    color: var(--gray-700);
    margin-bottom: 8px;
}
.form-group .req { color: var(--danger); }
.form-control {
    width: 100%;
    border-radius: 10px;
    border: 1px solid var(--gray-200);
    padding: 10px 14px;
    font-size: 14px;
    transition: all 0.2s ease;
    background: #fff;
}
.form-control:focus {
    border-color: var(--primary);
    box-shadow: 0 0 0 3px rgba(67,97,238,0.1);
    outline: none;
}

/* Buttons */
.btn-modern {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    padding: 11px 20px;
    border-radius: 12px;
    font-weight: 600;
    font-size: 14px;
    border: none;
    cursor: pointer;
    transition: all 0.25s ease;
    text-decoration: none;
    line-height: 1;
}
.btn-modern:disabled { opacity: 0.6; cursor: not-allowed; }
.btn-primary-modern {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: #fff;
    box-shadow: 0 4px 12px rgba(102,126,234,0.35);
}
.btn-primary-modern:hover:not(:disabled) {
    transform: translateY(-2px);
    box-shadow: 0 6px 20px rgba(102,126,234,0.45);
}
.btn-success-modern { background: rgba(16,185,129,0.12); color: var(--success); }
.btn-success-modern:hover { background: rgba(16,185,129,0.2); }
.btn-info-modern { background: rgba(14,165,233,0.12); color: var(--info); }
.btn-info-modern:hover { background: rgba(14,165,233,0.2); }
.btn-danger-modern { background: rgba(239,68,68,0.1); color: var(--danger); }
.btn-danger-modern:hover { background: rgba(239,68,68,0.18); }
.btn-outline-modern {
    background: #fff;
    border: 1px solid var(--gray-200);
    color: var(--gray-700);
}
.btn-outline-modern:hover { border-color: var(--primary); color: var(--primary); }
.btn-ghost {
    background: rgba(255,255,255,0.15);
    color: #fff;
    border: 1px solid rgba(255,255,255,0.25);
    backdrop-filter: blur(10px);
}
.btn-ghost:hover { background: rgba(255,255,255,0.25); color: #fff; }
.btn-row { display: flex; gap: 10px; flex-wrap: wrap; }

/* Profile Upload */
.profile-upload-container { cursor: pointer; }
.profile-preview {
    width: 180px; height: 180px;
    margin: 0 auto;
    border-radius: 50%;
    overflow: hidden;
    border: 3px solid var(--gray-200);
    background: var(--gray-50);
    cursor: pointer;
    transition: all 0.3s ease;
    display: flex; align-items: center; justify-content: center;
}
.profile-preview:hover { border-color: var(--primary); transform: scale(1.02); }
.profile-image { width: 100%; height: 100%; object-fit: cover; }
.profile-placeholder {
    width: 100%; height: 100%;
    display: flex; flex-direction: column;
    align-items: center; justify-content: center;
    background: linear-gradient(135deg, #f5f7fa, #e9ecef);
}

/* Spin */
@keyframes spin { from { transform: rotate(0deg); } to { transform: rotate(360deg); } }
.spin { animation: spin 1s linear infinite; }

/* Responsive */
@media (max-width: 768px) {
    .hero-content { flex-direction: column; align-items: flex-start; }
    .hero-title { font-size: 20px; }
}
</style>