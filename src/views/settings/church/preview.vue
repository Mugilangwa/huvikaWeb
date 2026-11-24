<template>
    <div class="layout-px-spacing church-dashboard m-4">
        <teleport to="#breadcrumb">
            <ul class="navbar-nav flex-row">
                <li>
                    <div class="page-header">
                        <nav class="breadcrumb-one" aria-label="breadcrumb">
                            <ol class="breadcrumb">
                                <li class="breadcrumb-item"><a href="javascript:;">Church</a></li>
                                <li class="breadcrumb-item active"><span>{{ church.name }}</span></li>
                            </ol>
                        </nav>
                    </div>
                </li>
            </ul>
        </teleport>

        <!-- Hero Profile Header -->
        <div class="hero-header mb-4">
            <div class="hero-content">
                <div class="hero-text">
                    <div class="hero-avatar">
                        <img :src="getChurchImage(church)" :alt="church.name" />
                    </div>
                    <div class="hero-info">
                        <h1 class="hero-title">{{ church.name }}</h1>
                        <div class="hero-meta">
                            <span class="meta-item">
                                <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                    <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"></path>
                                    <circle cx="12" cy="10" r="3"></circle>
                                </svg>
                                {{ church.location_name || 'Location not set' }}
                            </span>
                            <span class="meta-item">
                                <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                    <path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72c.127.96.362 1.903.7 2.81a2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45c.907.338 1.85.573 2.81.7A2 2 0 0 1 22 16.92z"></path>
                                </svg>
                                {{ church.phone || 'No phone' }}
                            </span>
                            <span class="meta-item code-pill">{{ church.code }}</span>
                        </div>
                    </div>
                </div>
                <router-link :to="'/parishi/edit/' + church.id" class="btn-modern btn-ghost">
                    <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                        <path d="M17 3a2.828 2.828 0 1 1 4 4L7.5 20.5 2 22l1.5-5.5L17 3z"></path>
                    </svg>
                    <span>Edit Church</span>
                </router-link>
            </div>
        </div>

        <!-- Statistics Cards -->
        <div class="row mb-4">
            <div class="col-md-3 col-6 mb-3">
                <div class="stat-card stat-primary">
                    <div class="stat-icon">
                        <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                            <path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"></path>
                            <circle cx="9" cy="7" r="4"></circle>
                            <path d="M23 21v-2a4 4 0 0 0-3-3.87"></path>
                            <path d="M16 3.13a4 4 0 0 1 0 7.75"></path>
                        </svg>
                    </div>
                    <div class="stat-info">
                        <span class="stat-value">{{ stats.members }}</span>
                        <span class="stat-label">Total Members</span>
                    </div>
                </div>
            </div>
            <div class="col-md-3 col-6 mb-3">
                <div class="stat-card stat-success">
                    <div class="stat-icon">
                        <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                            <rect x="2" y="7" width="20" height="14" rx="2" ry="2"></rect>
                            <path d="M16 21V5a2 2 0 0 0-2-2h-4a2 2 0 0 0-2 2v16"></path>
                        </svg>
                    </div>
                    <div class="stat-info">
                        <span class="stat-value">{{ stats.departments }}</span>
                        <span class="stat-label">Departments</span>
                    </div>
                </div>
            </div>
            <div class="col-md-3 col-6 mb-3">
                <div class="stat-card stat-warning">
                    <div class="stat-icon">
                        <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                            <path d="M12 2a10 10 0 1 0 10 10 4 4 0 0 1-5-5 4 4 0 0 1-5-5"></path>
                            <path d="M8.5 8.5L12 12"></path>
                            <path d="M16 8.5L12 12"></path>
                        </svg>
                    </div>
                    <div class="stat-info">
                        <span class="stat-value">{{ stats.groups }}</span>
                        <span class="stat-label">Groups</span>
                    </div>
                </div>
            </div>
            <div class="col-md-3 col-6 mb-3">
                <div class="stat-card stat-danger">
                    <div class="stat-icon">
                        <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                            <path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"></path>
                            <circle cx="12" cy="7" r="4"></circle>
                            <line x1="18" y1="8" x2="22" y2="8"></line>
                            <line x1="20" y1="6" x2="20" y2="10"></line>
                        </svg>
                    </div>
                    <div class="stat-info">
                        <span class="stat-value">{{ stats.leadership }}</span>
                        <span class="stat-label">Leadership</span>
                    </div>
                </div>
            </div>
        </div>

        <!-- Quick Actions -->
        <div class="modern-panel mb-4">
            <div class="panel-header">
                <h5>Quick Actions</h5>
                <p>Jump to management sections</p>
            </div>
            <div class="panel-body">
                <div class="quick-actions">
                    <router-link :to="'/church/' + church.id + '/departments'" class="quick-action qa-primary">
                        <div class="qa-icon">
                            <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                <rect x="2" y="7" width="20" height="14" rx="2" ry="2"></rect>
                                <path d="M16 21V5a2 2 0 0 0-2-2h-4a2 2 0 0 0-2 2v16"></path>
                            </svg>
                        </div>
                        <span>Departments</span>
                    </router-link>
                    <router-link :to="'/church/' + church.id + '/leadership'" class="quick-action qa-success">
                        <div class="qa-icon">
                            <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                <path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"></path>
                                <circle cx="12" cy="7" r="4"></circle>
                            </svg>
                        </div>
                        <span>Leadership</span>
                    </router-link>
                    <router-link :to="'/church/' + church.id + '/members'" class="quick-action qa-info">
                        <div class="qa-icon">
                            <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                <path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"></path>
                                <circle cx="9" cy="7" r="4"></circle>
                                <path d="M23 21v-2a4 4 0 0 0-3-3.87"></path>
                                <path d="M16 3.13a4 4 0 0 1 0 7.75"></path>
                            </svg>
                        </div>
                        <span>Members</span>
                    </router-link>
                    <router-link :to="'/church/' + church.id + '/groups'" class="quick-action qa-warning">
                        <div class="qa-icon">
                            <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                <path d="M12 2a10 10 0 1 0 10 10 4 4 0 0 1-5-5 4 4 0 0 1-5-5"></path>
                                <path d="M8.5 8.5L12 12"></path>
                                <path d="M16 8.5L12 12"></path>
                            </svg>
                        </div>
                        <span>Groups</span>
                    </router-link>
                    <router-link :to="'/church/' + church.id + '/attachments'" class="quick-action qa-secondary">
                        <div class="qa-icon">
                            <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                <path d="M13 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V9z"></path>
                                <polyline points="13 2 13 9 20 9"></polyline>
                            </svg>
                        </div>
                        <span>Attachments</span>
                    </router-link>
                </div>
            </div>
        </div>

        <!-- Recent Lists -->
        <div class="row">
            <div class="col-md-6 mb-4">
                <div class="modern-panel h-100">
                    <div class="panel-header flex-row-between">
                        <div>
                            <h5>Recent Departments</h5>
                            <p>Latest department records</p>
                        </div>
                        <router-link :to="'/church/' + church.id + '/departments'" class="btn-modern btn-outline-modern btn-sm">View All</router-link>
                    </div>
                    <div class="panel-body p-0">
                        <div class="table-responsive">
                            <table class="table modern-table mb-0">
                                <thead>
                                    <tr><th>Name</th><th>Status</th><th class="text-end">Actions</th></tr>
                                </thead>
                                <tbody>
                                    <tr v-for="dept in recentDepartments" :key="dept.id">
                                        <td class="fw-semibold">{{ dept.name }}</td>
                                        <td>
                                            <span class="status-pill" :class="dept.is_active ? 'status-active' : 'status-inactive'">
                                                <span class="status-indicator"></span>
                                                {{ dept.is_active ? 'Active' : 'Inactive' }}
                                            </span>
                                        </td>
                                        <td class="text-end">
                                            <router-link :to="'/church/' + church.id + '/departments/edit/' + dept.id" class="action-btn action-edit">
                                                <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                                    <path d="M17 3a2.828 2.828 0 1 1 4 4L7.5 20.5 2 22l1.5-5.5L17 3z"></path>
                                                </svg>
                                            </router-link>
                                        </td>
                                    </tr>
                                    <tr v-if="recentDepartments.length === 0">
                                        <td colspan="3" class="text-center text-muted py-4">No departments yet</td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>

            <div class="col-md-6 mb-4">
                <div class="modern-panel h-100">
                    <div class="panel-header flex-row-between">
                        <div>
                            <h5>Recent Leadership</h5>
                            <p>Latest leadership records</p>
                        </div>
                        <router-link :to="'/church/' + church.id + '/leadership'" class="btn-modern btn-outline-modern btn-sm">View All</router-link>
                    </div>
                    <div class="panel-body p-0">
                        <div class="table-responsive">
                            <table class="table modern-table mb-0">
                                <thead>
                                    <tr><th>Name</th><th>Role / Position</th><th class="text-end">Actions</th></tr>
                                </thead>
                                <tbody>
                                    <tr v-for="leader in recentLeadership" :key="leader.id">
                                        <td class="fw-semibold">{{ leader.leader_name }}</td>
                                        <td>
                                            <span class="code-badge">{{ leader.role_name }} {{ leader.position_name }}</span>
                                        </td>
                                        <td class="text-end">
                                            <router-link :to="'/church/' + church.id + '/leadership/edit/' + leader.id" class="action-btn action-edit">
                                                <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                                    <path d="M17 3a2.828 2.828 0 1 1 4 4L7.5 20.5 2 22l1.5-5.5L17 3z"></path>
                                                </svg>
                                            </router-link>
                                        </td>
                                    </tr>
                                    <tr v-if="recentLeadership.length === 0">
                                        <td colspan="3" class="text-center text-muted py-4">No leadership yet</td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>

            <div class="col-md-6 mb-4">
                <div class="modern-panel h-100">
                    <div class="panel-header flex-row-between">
                        <div>
                            <h5>Recent Members</h5>
                            <p>Latest member records</p>
                        </div>
                        <router-link :to="'/church/' + church.id + '/members'" class="btn-modern btn-outline-modern btn-sm">View All</router-link>
                    </div>
                    <div class="panel-body p-0">
                        <div class="table-responsive">
                            <table class="table modern-table mb-0">
                                <thead>
                                    <tr><th>Name</th><th>Membership No</th><th class="text-end">Actions</th></tr>
                                </thead>
                                <tbody>
                                    <tr v-for="member in recentMembers" :key="member.id">
                                        <td class="fw-semibold">{{ member.full_name }}</td>
                                        <td><span class="code-badge">{{ member.membership_number }}</span></td>
                                        <td class="text-end">
                                            <router-link :to="'/church/' + church.id + '/members/edit/' + member.id" class="action-btn action-edit">
                                                <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                                    <path d="M17 3a2.828 2.828 0 1 1 4 4L7.5 20.5 2 22l1.5-5.5L17 3z"></path>
                                                </svg>
                                            </router-link>
                                        </td>
                                    </tr>
                                    <tr v-if="recentMembers.length === 0">
                                        <td colspan="3" class="text-center text-muted py-4">No members yet</td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>

            <div class="col-md-6 mb-4">
                <div class="modern-panel h-100">
                    <div class="panel-header flex-row-between">
                        <div>
                            <h5>Recent Groups</h5>
                            <p>Latest group records</p>
                        </div>
                        <router-link :to="'/church/' + church.id + '/groups'" class="btn-modern btn-outline-modern btn-sm">View All</router-link>
                    </div>
                    <div class="panel-body p-0">
                        <div class="table-responsive">
                            <table class="table modern-table mb-0">
                                <thead>
                                    <tr><th>Name</th><th>Department</th><th class="text-end">Actions</th></tr>
                                </thead>
                                <tbody>
                                    <tr v-for="group in recentGroups" :key="group.id">
                                        <td class="fw-semibold">{{ group.name }}</td>
                                        <td><span class="code-badge">{{ group.department_name }}</span></td>
                                        <td class="text-end">
                                            <router-link :to="'/church/' + church.id + '/groups/edit/' + group.id" class="action-btn action-edit">
                                                <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                                    <path d="M17 3a2.828 2.828 0 1 1 4 4L7.5 20.5 2 22l1.5-5.5L17 3z"></path>
                                                </svg>
                                            </router-link>
                                        </td>
                                    </tr>
                                    <tr v-if="recentGroups.length === 0">
                                        <td colspan="3" class="text-center text-muted py-4">No groups yet</td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import { axiosInstance } from '@/services/axios';
import { useMeta } from '@/composables/use-meta';

useMeta({ title: 'Church Dashboard' });

const route = useRoute();
const churchId = route.params.id;

const church = ref({
    id: null, name: '', code: '', location_name: '', phone: '', profile_image: null
});

const stats = ref({ members: 0, departments: 0, groups: 0, leadership: 0 });
const recentDepartments = ref([]);
const recentLeadership = ref([]);
const recentMembers = ref([]);
const recentGroups = ref([]);

const getChurchImage = (church) => {
    if (church.profile_image && church.profile_image !== 'null') {
        if (church.profile_image.startsWith('http')) return church.profile_image;
        const baseUrl = process.env.VUE_APP_API_URL || 'http://127.0.0.1:8000';
        return `${baseUrl}${church.profile_image}`;
    }
    return '@/assets/img/profile.png';
};

const loadCompleteChurchData = async () => {
    try {
        const response = await axiosInstance.get(`/churches/${churchId}/`);
        const data = response.data.data;
        church.value = data;
        stats.value = {
            members: data.stats.total_members,
            departments: data.stats.total_departments,
            groups: data.stats.total_groups,
            leadership: data.stats.total_leadership
        };
        recentDepartments.value = data.departments.slice(0, 5);
        recentMembers.value = data.members.slice(0, 5);
        recentLeadership.value = data.leadership?.slice(0, 5) || [];
        const allGroups = data.departments.flatMap(dept => dept.groups || []);
        recentGroups.value = allGroups.slice(0, 5);
    } catch (error) {
        console.error('Error loading church data:', error);
    }
};

onMounted(() => loadCompleteChurchData());
</script>

<style scoped>
.church-dashboard {
    --primary: #4361ee;
    --primary-dark: #3a0ca3;
    --success: #10b981;
    --info: #0ea5e9;
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
.hero-text { display: flex; align-items: center; gap: 20px; }
.hero-avatar {
    width: 88px; height: 88px;
    border-radius: 50%;
    overflow: hidden;
    border: 3px solid rgba(255,255,255,0.35);
    box-shadow: 0 4px 16px rgba(0,0,0,0.2);
    flex-shrink: 0;
}
.hero-avatar img { width: 100%; height: 100%; object-fit: cover; }
.hero-title { font-size: 26px; font-weight: 700; color: #fff; margin: 0 0 8px 0; letter-spacing: -0.3px; }
.hero-meta { display: flex; flex-wrap: wrap; gap: 14px; align-items: center; }
.meta-item {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    color: rgba(255,255,255,0.9);
    font-size: 13px;
}
.meta-item svg { opacity: 0.8; }
.code-pill {
    background: rgba(255,255,255,0.2);
    padding: 4px 12px;
    border-radius: 20px;
    font-weight: 600;
    letter-spacing: 0.5px;
    border: 1px solid rgba(255,255,255,0.3);
}

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
.panel-header.flex-row-between { display: flex; justify-content: space-between; align-items: center; gap: 12px; }
.panel-body { padding: 24px; }

/* Stat Cards */
.stat-card {
    background: #fff;
    border-radius: 14px;
    padding: 20px;
    display: flex;
    align-items: center;
    gap: 14px;
    box-shadow: var(--shadow-sm);
    border: 1px solid var(--gray-200);
    transition: all 0.25s ease;
}
.stat-card:hover { transform: translateY(-3px); box-shadow: var(--shadow-md); }
.stat-icon {
    width: 48px; height: 48px;
    border-radius: 12px;
    display: flex; align-items: center; justify-content: center;
    flex-shrink: 0;
}
.stat-primary .stat-icon { background: rgba(67,97,238,0.1); color: var(--primary); }
.stat-success .stat-icon { background: rgba(16,185,129,0.1); color: var(--success); }
.stat-warning .stat-icon { background: rgba(245,158,11,0.1); color: var(--warning); }
.stat-danger .stat-icon { background: rgba(239,68,68,0.1); color: var(--danger); }
.stat-info { display: flex; flex-direction: column; }
.stat-value { font-size: 24px; font-weight: 700; color: var(--gray-900); line-height: 1.1; }
.stat-label { font-size: 12px; color: var(--gray-500); font-weight: 500; margin-top: 2px; }

/* Quick Actions */
.quick-actions { display: flex; flex-wrap: wrap; gap: 12px; }
.quick-action {
    display: flex;
    align-items: center;
    gap: 10px;
    padding: 12px 18px;
    border-radius: 12px;
    font-weight: 600;
    font-size: 13px;
    text-decoration: none;
    transition: all 0.2s ease;
    border: 1px solid transparent;
}
.quick-action:hover { transform: translateY(-2px); }
.qa-icon {
    width: 36px; height: 36px;
    border-radius: 10px;
    display: flex; align-items: center; justify-content: center;
    background: rgba(255,255,255,0.6);
}
.qa-primary { background: rgba(67,97,238,0.1); color: var(--primary); }
.qa-primary:hover { background: rgba(67,97,238,0.18); }
.qa-success { background: rgba(16,185,129,0.1); color: var(--success); }
.qa-success:hover { background: rgba(16,185,129,0.18); }
.qa-info { background: rgba(14,165,233,0.1); color: var(--info); }
.qa-info:hover { background: rgba(14,165,233,0.18); }
.qa-warning { background: rgba(245,158,11,0.1); color: var(--warning); }
.qa-warning:hover { background: rgba(245,158,11,0.18); }
.qa-secondary { background: rgba(107,114,128,0.1); color: var(--gray-700); }
.qa-secondary:hover { background: rgba(107,114,128,0.18); }

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
.btn-sm { padding: 8px 14px; font-size: 12px; }
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

/* Modern Table */
.modern-table thead th {
    background: var(--gray-50);
    padding: 12px 16px;
    font-weight: 600;
    font-size: 11px;
    text-transform: uppercase;
    letter-spacing: 0.5px;
    color: var(--gray-500);
    border-bottom: 1px solid var(--gray-200);
    border-top: none;
}
.modern-table tbody td {
    padding: 14px 16px;
    vertical-align: middle;
    border-bottom: 1px solid var(--gray-100);
    font-size: 13px;
}
.modern-table tbody tr:last-child td { border-bottom: none; }
.modern-table tbody tr:hover { background: var(--gray-50); }

/* Status */
.status-pill {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    padding: 5px 12px;
    border-radius: 20px;
    font-size: 11px;
    font-weight: 600;
}
.status-active { background: rgba(16,185,129,0.12); color: var(--success); }
.status-inactive { background: rgba(239,68,68,0.1); color: var(--danger); }
.status-indicator {
    width: 6px; height: 6px;
    border-radius: 50%;
    background: currentColor;
}

/* Code badge */
.code-badge {
    display: inline-block;
    padding: 4px 10px;
    background: linear-gradient(135deg, rgba(67,97,238,0.1), rgba(118,75,162,0.1));
    color: var(--primary);
    border-radius: 8px;
    font-size: 11px;
    font-weight: 600;
    border: 1px solid rgba(67,97,238,0.15);
}

/* Action buttons */
.action-btn {
    width: 30px; height: 30px;
    border-radius: 8px;
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
.action-btn:hover { transform: translateY(-2px); border-color: transparent; }
.action-edit:hover { background: rgba(16,185,129,0.1); color: var(--success); }

/* Responsive */
@media (max-width: 768px) {
    .hero-content { flex-direction: column; align-items: flex-start; }
    .hero-title { font-size: 20px; }
    .hero-text { flex-direction: column; align-items: flex-start; }
}
</style>