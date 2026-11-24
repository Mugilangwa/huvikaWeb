<template>
    <div class="layout-px-spacing church-list-page mt-4">
        <teleport to="#breadcrumb">
            <ul class="navbar-nav flex-row">
                <li>
                    <div class="page-header">
                        <nav class="breadcrumb-one" aria-label="breadcrumb">
                            <ol class="breadcrumb">
                                <li class="breadcrumb-item"><a href="javascript:;">Churches</a></li>
                                <li class="breadcrumb-item active"><span>Church List</span></li>
                            </ol>
                        </nav>
                    </div>
                </li>
            </ul>
        </teleport>

        <!-- Modern Hero Header -->
        <div class="hero-header mb-4">
            <div class="hero-content">
                <div class="hero-text">
                    <div class="hero-icon">
                        <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
                            <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"></path>
                            <polyline points="22,6 12,13 2,6"></polyline>
                        </svg>
                    </div>
                    <div>
                        <h1 class="hero-title">Church Management</h1>
                        <p class="hero-subtitle">Manage all churches, their locations and details</p>
                    </div>
                </div>
                <div class="hero-stats">
                    <div class="stat-card stat-primary">
                        <div class="stat-icon">
                            <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                <path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"></path>
                                <circle cx="9" cy="7" r="4"></circle>
                                <path d="M23 21v-2a4 4 0 0 0-3-3.87"></path>
                                <path d="M16 3.13a4 4 0 0 1 0 7.75"></path>
                            </svg>
                        </div>
                        <div class="stat-info">
                            <span class="stat-value">{{ totalChurches }}</span>
                            <span class="stat-label">Total Churches</span>
                        </div>
                    </div>
                    <div class="stat-card stat-success">
                        <div class="stat-icon">
                            <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                <path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"></path>
                                <polyline points="22 4 12 14.01 9 11.01"></polyline>
                            </svg>
                        </div>
                        <div class="stat-info">
                            <span class="stat-value">{{ activeChurches }}</span>
                            <span class="stat-label">Active</span>
                        </div>
                    </div>
                    <div class="stat-card stat-warning">
                        <div class="stat-icon">
                            <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                <rect x="3" y="3" width="18" height="18" rx="2" ry="2"></rect>
                                <circle cx="8.5" cy="8.5" r="1.5"></circle>
                                <polyline points="21 15 16 10 5 21"></polyline>
                            </svg>
                        </div>
                        <div class="stat-info">
                            <span class="stat-value">{{ churchesWithImages }}</span>
                            <span class="stat-label">With Images</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Main Content -->
        <div class="row">
            <div class="col-12">
                <div class="modern-panel">
                    <Loader v-if="isLoading" />
                    <div v-else class="custom-table p-4">
                        <v-client-table :data="items" :columns="columns" :options="table_option">
                            <template #beforeFilter>
                                <div class="table-actions">
                                    <router-link to="/parishi/add" class="btn-modern btn-primary-modern">
                                        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
                                            <line x1="12" y1="5" x2="12" y2="19"></line>
                                            <line x1="5" y1="12" x2="19" y2="12"></line>
                                        </svg>
                                        <span>Add New Church</span>
                                    </router-link>
                                </div>
                            </template>

                            <template #id="props">
                                <div class="modern-checkbox">
                                    <input type="checkbox" :id="'chk' + props.row.id" @change="toggleSelection(props.row.id)" />
                                    <label :for="'chk' + props.row.id"></label>
                                </div>
                            </template>

                            <template #name="props">
                                <div class="church-cell">
                                    <div class="avatar-wrapper">
                                        <img
                                            :src="getChurchImage(props.row)"
                                            :alt="props.row.name"
                                            @error="handleImageError(props.row)"
                                            class="church-avatar"
                                        />
                                        <span class="status-dot" :class="props.row.is_active ? 'online' : 'offline'"></span>
                                    </div>
                                    <div class="church-details">
                                        <span class="church-name">{{ props.row.name }}</span>
                                        <span class="church-alias">{{ props.row.aka || 'No alias' }}</span>
                                    </div>
                                </div>
                            </template>

                            <template #code="props">
                                <span class="code-badge">{{ props.row.code }}</span>
                            </template>

                            <template #location="props">
                                <div class="info-cell">
                                    <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                        <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"></path>
                                        <circle cx="12" cy="10" r="3"></circle>
                                    </svg>
                                    <span>{{ props.row.location_name || 'Not assigned' }}</span>
                                </div>
                            </template>

                            <template #phone="props">
                                <div class="info-cell">
                                    <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                        <path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72c.127.96.362 1.903.7 2.81a2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45c.907.338 1.85.573 2.81.7A2 2 0 0 1 22 16.92z"></path>
                                    </svg>
                                    <span>{{ props.row.phone || 'No phone' }}</span>
                                </div>
                            </template>

                            <template #status="props">
                                <button
                                    class="status-pill"
                                    :class="props.row.is_active ? 'status-active' : 'status-inactive'"
                                    @click="toggleStatus(props.row)"
                                >
                                    <span class="status-indicator"></span>
                                    {{ props.row.is_active ? 'Active' : 'Inactive' }}
                                </button>
                            </template>

                            <template #actions="props">
                                <div class="action-group">
                                    <router-link :to="'/parishi/view/' + props.row.id" class="action-btn action-view" title="View">
                                        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                            <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"></path>
                                            <circle cx="12" cy="12" r="3"></circle>
                                        </svg>
                                    </router-link>
                                    <router-link :to="'/parishi/edit/' + props.row.id" class="action-btn action-edit" title="Edit">
                                        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                            <path d="M17 3a2.828 2.828 0 1 1 4 4L7.5 20.5 2 22l1.5-5.5L17 3z"></path>
                                        </svg>
                                    </router-link>
                                    <button class="action-btn action-delete" @click="deleteChurch(props.row)" title="Delete">
                                        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                            <polyline points="3 6 5 6 21 6"></polyline>
                                            <path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"></path>
                                        </svg>
                                    </button>
                                </div>
                            </template>
                        </v-client-table>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue';
import { axiosInstance } from '@/services/axios';
import { useMeta } from '@/composables/use-meta';
import { useNotification } from '@/composables/swal';
import Loader from '@/components/plugins/loader.vue';

const { showAlert } = useNotification();
useMeta({ title: 'Church List' });

const items = ref([]);
const selectedIds = ref([]);
const isLoading = ref(false);
const DEFAULT_CHURCH_IMAGE = '../assets/img/klpt.png';

const columns = ref(['id', 'name', 'code', 'location', 'phone', 'status', 'actions']);

const table_option = ref({
    perPage: 10,
    perPageValues: [10, 25, 50, 100],
    skin: 'table modern-table',
    columnsClasses: { actions: 'text-center', name: 'col-name' },
    texts: {
        filterPlaceholder: '🔍  Search churches by name, code...',
        filter: '',
        count: 'Showing {from} to {to} of {count} churches'
    },
    sortable: ['name', 'code', 'location', 'phone'],
    filterable: ['name', 'code', 'location_name'],
    headings: {
        id: '',
        name: 'Church',
        code: 'Code',
        location: 'Location',
        phone: 'Contact',
        status: 'Status',
        actions: 'Actions'
    }
});

const totalChurches = computed(() => items.value.length);
const activeChurches = computed(() => items.value.filter(c => c.is_active).length);
const churchesWithImages = computed(() => items.value.filter(c => c.profile_image).length);

const getChurchImage = (church) => {
    if (church.profile_image && church.profile_image !== 'null') {
        if (church.profile_image.startsWith('http')) return church.profile_image;
        const baseUrl = process.env.VUE_APP_API_URL || 'http://127.0.0.1:8000';
        return `${baseUrl}${church.profile_image}`;
    }
    return DEFAULT_CHURCH_IMAGE;
};

const handleImageError = (church) => {
    church.profile_image = null;
    const imgElement = event.target;
    if (imgElement) imgElement.src = DEFAULT_CHURCH_IMAGE;
};

const loadChurches = async () => {
    isLoading.value = true;
    try {
        const response = await axiosInstance.get('/churches/');
        items.value = response.data;
    } catch (error) {
        console.error('Error loading churches:', error);
    } finally {
        isLoading.value = false;
    }
};

const toggleSelection = (id) => {
    if (selectedIds.value.includes(id)) {
        selectedIds.value = selectedIds.value.filter(i => i !== id);
    } else {
        selectedIds.value.push(id);
    }
};

const toggleStatus = async (church) => {
    try {
        await axiosInstance.patch(`/churches/${church.id}/`, { is_active: !church.is_active });
        await loadChurches();
    } catch (error) {
        console.error('Error toggling status:', error);
    }
};

const deleteChurch = async (church) => {
    if (confirm(`Delete church "${church.name}"?`)) {
        try {
            await axiosInstance.delete(`/churches/${church.id}/`);
            await loadChurches();
        } catch (error) {
            console.error('Error deleting church:', error);
        }
    }
};

onMounted(() => loadChurches());
</script>

<style scoped>
/* ========== PAGE WRAPPER ========== */
.church-list-page {
    --primary: #4361ee;
    --primary-dark: #3a0ca3;
    --success: #10b981;
    --warning: #f59e0b;
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

/* ========== HERO HEADER ========== */
.hero-header {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    border-radius: var(--radius);
    padding: 32px;
    position: relative;
    overflow: hidden;
    box-shadow: var(--shadow-lg);
}

.hero-header::before {
    content: '';
    position: absolute;
    top: -50%;
    right: -10%;
    width: 400px;
    height: 400px;
    background: rgba(255,255,255,0.08);
    border-radius: 50%;
}

.hero-header::after {
    content: '';
    position: absolute;
    bottom: -60%;
    left: -5%;
    width: 300px;
    height: 300px;
    background: rgba(255,255,255,0.05);
    border-radius: 50%;
}

.hero-content {
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
    gap: 24px;
    position: relative;
    z-index: 1;
}

.hero-text {
    display: flex;
    align-items: center;
    gap: 20px;
}

.hero-icon {
    width: 64px;
    height: 64px;
    background: rgba(255,255,255,0.15);
    backdrop-filter: blur(10px);
    border-radius: 18px;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #fff;
    border: 1px solid rgba(255,255,255,0.2);
}

.hero-title {
    font-size: 28px;
    font-weight: 700;
    color: #fff;
    margin: 0 0 4px 0;
    letter-spacing: -0.5px;
}

.hero-subtitle {
    color: rgba(255,255,255,0.85);
    margin: 0;
    font-size: 14px;
}

/* ========== STAT CARDS ========== */
.hero-stats {
    display: flex;
    gap: 12px;
    flex-wrap: wrap;
}

.stat-card {
    background: rgba(255,255,255,0.95);
    backdrop-filter: blur(10px);
    border-radius: 14px;
    padding: 16px 20px;
    display: flex;
    align-items: center;
    gap: 12px;
    min-width: 140px;
    box-shadow: var(--shadow-md);
    transition: transform 0.3s ease;
}

.stat-card:hover {
    transform: translateY(-3px);
}

.stat-icon {
    width: 40px;
    height: 40px;
    border-radius: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
}

.stat-primary .stat-icon {
    background: rgba(67,97,238,0.1);
    color: var(--primary);
}
.stat-success .stat-icon {
    background: rgba(16,185,129,0.1);
    color: var(--success);
}
.stat-warning .stat-icon {
    background: rgba(245,158,11,0.1);
    color: var(--warning);
}

.stat-info {
    display: flex;
    flex-direction: column;
}

.stat-value {
    font-size: 22px;
    font-weight: 700;
    color: var(--gray-900);
    line-height: 1.2;
}

.stat-label {
    font-size: 11px;
    color: var(--gray-500);
    font-weight: 500;
    text-transform: uppercase;
    letter-spacing: 0.5px;
}

/* ========== MODERN PANEL ========== */
.modern-panel {
    background: #fff;
    border-radius: var(--radius);
    box-shadow: var(--shadow-md);
    overflow: hidden;
    border: 1px solid var(--gray-200);
}

/* ========== TABLE ACTIONS ========== */
.table-actions {
    display: flex;
    gap: 10px;
    margin-bottom: 8px;
}

.btn-modern {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    padding: 11px 20px;
    border-radius: 12px;
    font-weight: 600;
    font-size: 14px;
    border: none;
    cursor: pointer;
    transition: all 0.25s ease;
    text-decoration: none;
}

.btn-primary-modern {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: #fff;
    box-shadow: 0 4px 12px rgba(102,126,234,0.35);
}

.btn-primary-modern:hover {
    transform: translateY(-2px);
    box-shadow: 0 6px 20px rgba(102,126,234,0.45);
    color: #fff;
}

/* ========== TABLE STYLES ========== */
.custom-table :deep(.modern-table) {
    border-collapse: separate;
    border-spacing: 0;
}

.custom-table :deep(.modern-table thead th) {
    background: var(--gray-50);
    padding: 14px 16px;
    font-weight: 600;
    font-size: 12px;
    text-transform: uppercase;
    letter-spacing: 0.5px;
    color: var(--gray-500);
    border-bottom: 2px solid var(--gray-200);
    border-top: none;
}

.custom-table :deep(.modern-table tbody tr) {
    background: #fff;
    transition: background 0.2s ease;
}

.custom-table :deep(.modern-table tbody tr:hover) {
    background: var(--gray-50);
}

.custom-table :deep(.modern-table tbody td) {
    padding: 16px;
    vertical-align: middle;
    border-bottom: 1px solid var(--gray-100);
}

/* ========== CHURCH CELL ========== */
.church-cell {
    display: flex;
    align-items: center;
    gap: 14px;
}

.avatar-wrapper {
    position: relative;
    flex-shrink: 0;
}

.church-avatar {
    width: 46px;
    height: 46px;
    border-radius: 50%;
    object-fit: cover;
    border: 2px solid #fff;
    box-shadow: 0 2px 8px rgba(0,0,0,0.12);
    display: block;
}

.status-dot {
    position: absolute;
    bottom: 0;
    right: 0;
    width: 12px;
    height: 12px;
    border-radius: 50%;
    border: 2px solid #fff;
}

.status-dot.online { background: var(--success); }
.status-dot.offline { background: var(--gray-500); }

.church-details {
    display: flex;
    flex-direction: column;
    line-height: 1.3;
}

.church-name {
    font-weight: 600;
    color: var(--gray-900);
    font-size: 14px;
}

.church-alias {
    font-size: 12px;
    color: var(--gray-500);
}

/* ========== INFO CELL ========== */
.info-cell {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    color: var(--gray-700);
    font-size: 13px;
}

.info-cell svg {
    color: var(--gray-500);
    flex-shrink: 0;
}

/* ========== CODE BADGE ========== */
.code-badge {
    display: inline-block;
    padding: 5px 12px;
    background: linear-gradient(135deg, rgba(67,97,238,0.1), rgba(118,75,162,0.1));
    color: var(--primary);
    border-radius: 8px;
    font-size: 12px;
    font-weight: 600;
    letter-spacing: 0.3px;
    border: 1px solid rgba(67,97,238,0.15);
}

/* ========== STATUS PILL ========== */
.status-pill {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    padding: 6px 14px;
    border-radius: 20px;
    font-size: 12px;
    font-weight: 600;
    border: none;
    cursor: pointer;
    transition: all 0.2s ease;
}

.status-active {
    background: rgba(16,185,129,0.12);
    color: var(--success);
}

.status-active:hover {
    background: rgba(16,185,129,0.2);
}

.status-inactive {
    background: rgba(239,68,68,0.1);
    color: var(--danger);
}

.status-inactive:hover {
    background: rgba(239,68,68,0.18);
}

.status-indicator {
    width: 6px;
    height: 6px;
    border-radius: 50%;
    background: currentColor;
    box-shadow: 0 0 0 3px rgba(255,255,255,0.5);
}

/* ========== ACTION BUTTONS ========== */
.action-group {
    display: inline-flex;
    gap: 6px;
    justify-content: center;
}

.action-btn {
    width: 34px;
    height: 34px;
    border-radius: 10px;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    border: 1px solid var(--gray-200);
    background: #fff;
    color: var(--gray-700);
    cursor: pointer;
    transition: all 0.2s ease;
    text-decoration: none;
}

.action-btn:hover {
    transform: translateY(-2px);
    border-color: transparent;
}

.action-view:hover {
    background: rgba(67,97,238,0.1);
    color: var(--primary);
}

.action-edit:hover {
    background: rgba(16,185,129,0.1);
    color: var(--success);
}

.action-delete:hover {
    background: rgba(239,68,68,0.1);
    color: var(--danger);
}

/* ========== MODERN CHECKBOX ========== */
.modern-checkbox input[type="checkbox"] {
    display: none;
}

.modern-checkbox label {
    display: inline-block;
    width: 18px;
    height: 18px;
    border: 2px solid var(--gray-200);
    border-radius: 5px;
    cursor: pointer;
    position: relative;
    transition: all 0.2s ease;
}

.modern-checkbox input:checked + label {
    background: var(--primary);
    border-color: var(--primary);
}

.modern-checkbox input:checked + label::after {
    content: '';
    position: absolute;
    left: 4px;
    top: 1px;
    width: 5px;
    height: 10px;
    border: solid white;
    border-width: 0 2px 2px 0;
    transform: rotate(45deg);
}

/* ========== SEARCH FILTER ========== */
.custom-table :deep(.VueTables__search-field input) {
    border-radius: 10px;
    border: 1px solid var(--gray-200);
    padding: 10px 16px;
    font-size: 14px;
    transition: all 0.2s ease;
}

.custom-table :deep(.VueTables__search-field input:focus) {
    border-color: var(--primary);
    box-shadow: 0 0 0 3px rgba(67,97,238,0.1);
    outline: none;
}

/* ========== RESPONSIVE ========== */
@media (max-width: 768px) {
    .hero-content {
        flex-direction: column;
        align-items: flex-start;
    }
    .hero-stats {
        width: 100%;
    }
    .stat-card {
        flex: 1;
        min-width: 0;
    }
    .hero-title {
        font-size: 22px;
    }
}
</style>