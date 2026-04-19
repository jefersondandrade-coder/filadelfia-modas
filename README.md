Filadelphia Modas Mundial - Project TODOCompleted Features (from source project)FrontendHomepage with banners carousel, categories, daily offers, featured products, and campaignsProduct listing with filters (category, price, rating)Product detail page with image gallery, color/size selection, and cartShopping cart with discount coupon (test: FILADELPHIA10)Login/Signup formsAffiliate program with commission tableAdmin panel for managing products, banners, and campaignsMobile navigation with bottom navBackendMySQL database with products, banners, campaigns, and users tablesManus OAuth authenticationtRPC API for secure frontend-backend communicationAccess control (admin-only operations)CRUD operations for products, banners, and campaignsPending Tasks

import { defineConfig } from "vitest/config";
import path from "path";

const templateRoot = path.resolve(import.meta.dirname);

export default defineConfig({
  root: templateRoot,
  resolve: {
    alias: {
      "@": path.resolve(templateRoot, "client", "src"),
      "@shared": path.resolve(templateRoot, "shared"),
      "@assets": path.resolve(templateRoot, "attached_assets"),
    },
  },
  test: {
    environment: "node",
    include: ["server/**/*.test.ts", "server/**/*.spec.ts"],
  },
});

import { describe, it, expect, vi, beforeEach } from "vitest";
import { render, screen, waitFor } from "@testing-library/react";
import ProtectedAdminRoute from "./ProtectedAdminRoute";
import * as useAuthModule from "@/_core/hooks/useAuth";
import * as wouterModule from "wouter";

// Mock the useAuth hook
vi.mock("@/_core/hooks/useAuth", () => ({
  useAuth: vi.fn(),
}));

// Mock wouter
vi.mock("wouter", () => ({
  useLocation: vi.fn(),
}));

const mockSetLocation = vi.fn();

describe("ProtectedAdminRoute", () => {
  beforeEach(() => {
    vi.clearAllMocks();
    mockSetLocation.mockClear();
  });

  it("should show loading state while checking authentication", () => {
    vi.mocked(useAuthModule.useAuth).mockReturnValue({
      user: null,
      loading: true,
      error: null,
      isAuthenticated: false,
      refresh: vi.fn(),
      logout: vi.fn(),
    });

    vi.mocked(wouterModule.useLocation).mockReturnValue([
      "/admin",
      mockSetLocation,
    ]);

    render(
      <ProtectedAdminRoute>
        <div>Admin Content</div>
      </ProtectedAdminRoute>
    );

    expect(screen.getByText("Verificando acesso...")).toBeInTheDocument();
  });

  it("should redirect to login when user is not authenticated", async () => {
    vi.mocked(useAuthModule.useAuth).mockReturnValue({
      user: null,
      loading: false,
      error: null,
      isAuthenticated: false,
      refresh: vi.fn(),
      logout: vi.fn(),
    });

    vi.mocked(wouterModule.useLocation).mockReturnValue([
      "/admin",
      mockSetLocation,
    ]);

    render(
      <ProtectedAdminRoute>
        <div>Admin Content</div>
      </ProtectedAdminRoute>
    );

    await waitFor(() => {
      expect(mockSetLocation).toHaveBeenCalledWith("/login");
    });
  });

  it("should redirect to login when user email is not admin email", async () => {
    vi.mocked(useAuthModule.useAuth).mockReturnValue({
      user: {
        id: 1,
        openId: "user-123",
        email: "other@example.com",
        name: "Other User",
        loginMethod: "manus",
        role: "user",
        createdAt: new Date(),
        updatedAt: new Date(),
        lastSignedIn: new Date(),
      },
      loading: false,
      error: null,
      isAuthenticated: true,
      refresh: vi.fn(),
      logout: vi.fn(),
    });

    vi.mocked(wouterModule.useLocation).mockReturnValue([
      "/admin",
      mockSetLocation,
    ]);

    render(
      <ProtectedAdminRoute>
        <div>Admin Content</div>
      </ProtectedAdminRoute>
    );

    await waitFor(() => {
      expect(mockSetLocation).toHaveBeenCalledWith("/login");
    });
  });

  it("should render children when user is authenticated with admin email", async () => {
    vi.mocked(useAuthModule.useAuth).mockReturnValue({
      user: {
        id: 1,
        openId: "admin-123",
        email: "jeferson_d_andrade@hotmail.com",
        name: "Admin User",
        loginMethod: "manus",
        role: "admin",
        createdAt: new Date(),
        updatedAt: new Date(),
        lastSignedIn: new Date(),
      },
      loading: false,
      error: null,
      isAuthenticated: true,
      refresh: vi.fn(),
      logout: vi.fn(),
    });

    vi.mocked(wouterModule.useLocation).mockReturnValue([
      "/admin",
      mockSetLocation,
    ]);

    render(
      <ProtectedAdminRoute>
        <div>Admin Content</div>
      </ProtectedAdminRoute>
    );

    await waitFor(() => {
      expect(screen.getByText("Admin Content")).toBeInTheDocument();
    });

    expect(mockSetLocation).not.toHaveBeenCalled();
  });
});

// ============================================================
// FILADELPHIA MODAS MUNDIAL — App Router
// Style: Vibrant Brazilian Street Market
// Routes: Home, Products, ProductDetail, Cart, Login, Affiliates, Admin, Wishlist
// ============================================================
import { Toaster } from "@/components/ui/sonner";
import { TooltipProvider } from "@/components/ui/tooltip";
import NotFound from "@/pages/NotFound";
import { Route, Switch, useLocation } from "wouter";
import ErrorBoundary from "./components/ErrorBoundary";
import { ThemeProvider } from "./contexts/ThemeContext";
import { CartProvider } from "./contexts/CartContext";
import Header from "./components/Header";
import Footer from "./components/Footer";
import BottomNav from "./components/BottomNav";
import Home from "./pages/Home";
import Products from "./pages/Products";
import ProductDetail from "./pages/ProductDetail";
import Cart from "./pages/Cart";
import Login from "./pages/Login";
import Affiliates from "./pages/Affiliates";
import Admin from "./pages/Admin";
import Wishlist from "./pages/Wishlist";
import Checkout from "./pages/Checkout";
import ProtectedAdminRoute from "./components/ProtectedAdminRoute";

// Layout wrapper — excludes header/footer for admin and login pages
function Layout({ children }: { children: React.ReactNode }) {
  const [location] = useLocation();
  const isAdmin = location.startsWith("/admin");
  const isLogin = location === "/login";
  const isCheckout = location === "/checkout";

  if (isAdmin || isCheckout) {
    return <>{children}</>;
  }

  return (
    <div className="flex flex-col min-h-screen">
      <Header />
      <main className="flex-1">
        {children}
      </main>
      {!isLogin && <Footer />}
      <BottomNav />
    </div>
  );
}
function Router() {
  // make sure to consider if you need authentication for certain routes
  return (
    <Layout>
      <Switch>
        <Route path="/" component={Home} />
        <Route path="/produtos" component={Products} />
        <Route path="/produto/:id" component={ProductDetail} />
        <Route path="/carrinho" component={Cart} />
        <Route path="/login" component={Login} />
        <Route path="/afiliados" component={Affiliates} />
        <Route path="/favoritos" component={Wishlist} />
        <Route path="/checkout" component={Checkout} />
        <Route path="/admin">
          {() => (
            <ProtectedAdminRoute>
              <Admin />
            </ProtectedAdminRoute>
          )}
        </Route>
        <Route path="/404" component={NotFound} />
        <Route component={NotFound} />
      </Switch>
    </Layout>
  );
}

function App() {
  return (
    <ErrorBoundary>
      <ThemeProvider defaultTheme="light">
        <CartProvider>
          <TooltipProvider>
            <Toaster position="top-right" richColors />
            <Router />
          </TooltipProvider>
        </CartProvider>
      </ThemeProvider>
    </ErrorBoundary>
  );
}

import { useAuth } from "@/_core/hooks/useAuth";
import { useLocation } from "wouter";
import { useEffect } from "react";
import { Loader2 } from "lucide-react";

const ADMIN_EMAIL = "jeferson_d_andrade@hotmail.com";

interface ProtectedAdminRouteProps {
  children: React.ReactNode;
}

/**
 * ProtectedAdminRoute - Wrapper component that protects admin routes
 * Only allows access to users with the admin email (jeferson_d_andrade@hotmail.com)
 * Redirects unauthenticated users or non-admin users to /login
 */
export default function ProtectedAdminRoute({ children }: ProtectedAdminRouteProps) {
  const { user, loading } = useAuth();
  const [, setLocation] = useLocation();

  useEffect(() => {
    // While loading auth state, don't redirect yet
    if (loading) return;

    // If not authenticated, redirect to login
    if (!user) {
      setLocation("/login");
      return;
    }

    // If authenticated but not the admin email, redirect to login
    if (user.email !== ADMIN_EMAIL) {
      setLocation("/login");
      return;
    }
  }, [user, loading, setLocation]);

  // Show loading state while checking authentication
  if (loading) {
    return (
      <div className="flex items-center justify-center min-h-screen bg-background">
        <div className="flex flex-col items-center gap-4">
          <Loader2 className="w-8 h-8 animate-spin text-primary" />
          <p className="text-sm text-muted-foreground">Verificando acesso...</p>
        </div>
      </div>
    );
  }

  // If user is authenticated and is the admin, render children
  if (user && user.email === ADMIN_EMAIL) {
    return <>{children}</>;
  }

  // Fallback - should not reach here due to useEffect redirects
  return null;
}

UseAuth.ts

import { getLoginUrl } from "@/const";
import { trpc } from "@/lib/trpc";
import { TRPCClientError } from "@trpc/client";
import { useCallback, useEffect, useMemo } from "react";

type UseAuthOptions = {
  redirectOnUnauthenticated?: boolean;
  redirectPath?: string;
};

export function useAuth(options?: UseAuthOptions) {
  const { redirectOnUnauthenticated = false, redirectPath = getLoginUrl() } =
    options ?? {};
  const utils = trpc.useUtils();

  const meQuery = trpc.auth.me.useQuery(undefined, {
    retry: false,
    refetchOnWindowFocus: false,
  });

  const logoutMutation = trpc.auth.logout.useMutation({
    onSuccess: () => {
      utils.auth.me.setData(undefined, null);
    },
  });

  const logout = useCallback(async () => {
    try {
      await logoutMutation.mutateAsync();
    } catch (error: unknown) {
      if (
        error instanceof TRPCClientError &&
        error.data?.code === "UNAUTHORIZED"
      ) {
        return;
      }
      throw error;
    } finally {
      utils.auth.me.setData(undefined, null);
      await utils.auth.me.invalidate();
    }
  }, [logoutMutation, utils]);

  const state = useMemo(() => {
    localStorage.setItem(
      "manus-runtime-user-info",
      JSON.stringify(meQuery.data)
    );
    return {
      user: meQuery.data ?? null,
      loading: meQuery.isLoading || logoutMutation.isPending,
      error: meQuery.error ?? logoutMutation.error ?? null,
      isAuthenticated: Boolean(meQuery.data),
    };
  }, [
    meQuery.data,
    meQuery.error,
    meQuery.isLoading,
    logoutMutation.error,
    logoutMutation.isPending,
  ]);

  useEffect(() => {
    if (!redirectOnUnauthenticated) return;
    if (meQuery.isLoading || logoutMutation.isPending) return;
    if (state.user) return;
    if (typeof window === "undefined") return;
    if (window.location.pathname === redirectPath) return;

    window.location.href = redirectPath
  }, [
    redirectOnUnauthenticated,
    redirectPath,
    logoutMutation.isPending,
    meQuery.isLoading,
    state.user,
  ]);

  return {
    ...state,
    refresh: () => meQuery.refetch(),
    logout,
  };
}

Admin.tsx

// ============================================================
// FILADELPHIA MODAS MUNDIAL — Admin Panel (Profissional)
// Gerenciamento completo: Produtos, Banners, Campanhas, Pagamentos, Pedidos
// ============================================================
import { useState } from "react";
import {
  LayoutDashboard, Package, Image, Tag, DollarSign, ShoppingCart,
  Plus, Edit2, Trash2, Eye, EyeOff, Search, LogOut, X, Save,
  TrendingUp, Users, BarChart3, Star, Upload, ChevronDown
} from "lucide-react";
import { PRODUCTS, BANNERS, CAMPAIGNS, formatPrice } from "@/lib/data";
import { toast } from "sonner";
import { Link } from "wouter";

type AdminSection = "dashboard" | "products" | "banners" | "campaigns" | "payments" | "orders" | "transactions";

interface ProductForm {
  name: string;
  description: string;
  price: number;
  originalPrice?: number;
  category: string;
  image: string;
  colors: string[];
  sizes: string[];
  stock: number;
}

interface BannerForm {
  title: string;
  subtitle: string;
  image: string;
  link: string;
}

interface CampaignForm {
  title: string;
  subtitle: string;
  image: string;
  discount: number;
  productId: number;
  startDate: string;
  endDate: string;
}

export default function Admin() {
  const [section, setSection] = useState<AdminSection>("dashboard");
  const [sidebarOpen, setSidebarOpen] = useState(true);
  const [searchQuery, setSearchQuery] = useState("");
  const [editingProduct, setEditingProduct] = useState<ProductForm | null>(null);
  const [editingBanner, setEditingBanner] = useState<BannerForm | null>(null);
  const [editingCampaign, setEditingCampaign] = useState<CampaignForm | null>(null);
  const [showProductForm, setShowProductForm] = useState(false);
  const [showBannerForm, setShowBannerForm] = useState(false);
  const [showCampaignForm, setShowCampaignForm] = useState(false);
  const [newColor, setNewColor] = useState("");
  const [newSize, setNewSize] = useState("");

  // Payment config state
  const [pixKey, setPixKey] = useState("");
  const [pixKeyType, setPixKeyType] = useState<"cpf" | "email" | "cnpj">("cpf");
  const [mercadoPagoToken, setMercadoPagoToken] = useState("");
  const [mercadoPagoPublicKey, setMercadoPagoPublicKey] = useState("");

  const navItems = [
    { id: "dashboard" as AdminSection, icon: LayoutDashboard, label: "Dashboard" },
    { id: "products" as AdminSection, icon: Package, label: "Produtos" },
    { id: "banners" as AdminSection, icon: Image, label: "Banners" },
    { id: "campaigns" as AdminSection, icon: Tag, label: "Campanhas" },
    { id: "payments" as AdminSection, icon: DollarSign, label: "Pagamentos" },
    { id: "orders" as AdminSection, icon: ShoppingCart, label: "Pedidos" },
    { id: "transactions" as AdminSection, icon: BarChart3, label: "Transações" },
  ];

  const stats = [
    { icon: DollarSign, label: "Receita do Mês", value: "R$ 48.320", change: "+12%", color: "text-green-600", bg: "bg-green-50" },
    { icon: ShoppingCart, label: "Pedidos", value: "342", change: "+8%", color: "text-blue-600", bg: "bg-blue-50" },
    { icon: Users, label: "Clientes", value: "1.284", change: "+23%", color: "text-purple-600", bg: "bg-purple-50" },
    { icon: TrendingUp, label: "Conversão", value: "3.4%", change: "+0.5%", color: "text-orange-600", bg: "bg-orange-50" },
  ];

  const handleSaveProduct = async (e: React.FormEvent) => {
    e.preventDefault();
    if (!editingProduct?.name || !editingProduct?.price) {
      toast.error("Preencha nome e preco");
      return;
    }
    try {
      const { addProduct } = await import("@/lib/firebaseService");
      const productData = {
        name: editingProduct.name,
        description: editingProduct.description || "",
        price: Number(editingProduct.price),
        discountPrice: editingProduct.originalPrice ? Number(editingProduct.originalPrice) : undefined,
        category: editingProduct.category || "Geral",
        colors: Array.isArray(editingProduct.colors) ? editingProduct.colors : [],
        sizes: Array.isArray(editingProduct.sizes) ? editingProduct.sizes : [],
        images: editingProduct.image ? [editingProduct.image] : [],
        stock: editingProduct.stock ?? 0,
        rating: 0,
        reviews: 0,
        isFeatured: false,
        isOffer: false,
        isBestSeller: false
      };
      await addProduct(productData);
      toast.success("Produto salvo com sucesso no Firebase!");
      setShowProductForm(false);
      setEditingProduct(null);
    } catch (error) {
      console.error("Erro ao salvar:", error);
      toast.error("Erro ao salvar o produto.");
    }
  };

  const handleSaveBanner = () => {
    if (!editingBanner?.title || !editingBanner?.image) {
      toast.error("Preencha título e imagem");
      return;
    }
    toast.success("Banner salvo com sucesso!");
    setShowBannerForm(false);
    setEditingBanner(null);
  };

  const handleSaveCampaign = () => {
    if (!editingCampaign?.title || !editingCampaign?.discount) {
      toast.error("Preencha título e desconto");
      return;
    }
    toast.success("Campanha salva com sucesso!");
    setShowCampaignForm(false);
    setEditingCampaign(null);
  };

  const handleSavePayments = () => {
    if (!pixKey && !mercadoPagoToken) {
      toast.error("Configure pelo menos um método de pagamento");
      return;
    }
    toast.success("Configurações de pagamento salvas!");
  };

  return (
    <div className="min-h-screen bg-gray-100 flex">
      {/* Sidebar */}
      <aside className={`${sidebarOpen ? "w-56" : "w-14"} bg-gray-900 flex-shrink-0 transition-all duration-300 flex flex-col`}>
        <div className="p-4 border-b border-gray-700 flex items-center gap-3">
          <div className="w-8 h-8 bg-orange-500 rounded-lg flex items-center justify-center flex-shrink-0">
            <span className="text-white font-black text-base">F</span>
          </div>
          {sidebarOpen && (
            <div>
              <div className="text-white font-black text-sm leading-tight">Filadelphia</div>
              <div className="text-orange-400 text-xs font-semibold">Admin</div>
            </div>
          )}
        </div>

        <nav className="flex-1 py-4">
          {navItems.map((item) => (
            <button
              key={item.id}
              onClick={() => setSection(item.id)}
              className={`w-full flex items-center gap-3 px-4 py-3 text-sm font-semibold transition-colors ${
                section === item.id
                  ? "bg-orange-500 text-white"
                  : "text-gray-400 hover:bg-gray-800 hover:text-white"
              }`}
            >
              <item.icon size={18} className="flex-shrink-0" />
              {sidebarOpen && <span>{item.label}</span>}
            </button>
          ))}
        </nav>

        <div className="p-4 border-t border-gray-700 space-y-2">
          <Link href="/">
            <button className="w-full flex items-center gap-3 text-gray-400 hover:text-white text-sm font-semibold transition-colors">
              <Eye size={18} />
              {sidebarOpen && "Ver Site"}
            </button>
          </Link>
          <button
            onClick={() => toast.info("Logout realizado")}
            className="w-full flex items-center gap-3 text-gray-400 hover:text-red-400 text-sm font-semibold transition-colors"
          >
            <LogOut size={18} />
            {sidebarOpen && "Sair"}
          </button>
        </div>
      </aside>

      {/* Main content */}
      <div className="flex-1 flex flex-col min-w-0">
        {/* Top bar */}
        <header className="bg-white border-b px-6 py-3 flex items-center justify-between">
          <div className="flex items-center gap-3">
            <button
              onClick={() => setSidebarOpen(!sidebarOpen)}
              className="text-gray-500 hover:text-gray-700"
            >
              <LayoutDashboard size={20} />
            </button>
            <h2 className="font-black text-gray-800 text-lg capitalize">
              {section === "dashboard" ? "Dashboard" :
               section === "products" ? "Gerenciar Produtos" :
               section === "banners" ? "Gerenciar Banners" :
               section === "campaigns" ? "Gerenciar Campanhas" :
               section === "payments" ? "Configurar Pagamentos" :
               section === "orders" ? "Pedidos" : "Transações"}
            </h2>
          </div>

          <div className="flex items-center gap-3">
            <div className="relative">
              <Search size={15} className="absolute left-3 top-1/2 -translate-y-1/2 text-gray-400" />
              <input
                type="text"
                value={searchQuery}
                onChange={(e) => setSearchQuery(e.target.value)}
                placeholder="Buscar..."
                className="pl-8 pr-4 py-2 border border-gray-200 rounded-lg text-sm outline-none focus:border-orange-400 w-48"
              />
            </div>
            <div className="w-8 h-8 bg-orange-500 rounded-full flex items-center justify-center">
              <span className="text-white font-black text-sm">A</span>
            </div>
          </div>
        </header>

        {/* Content */}
        <main className="flex-1 p-6 overflow-auto">
          {/* ===== DASHBOARD ===== */}
          {section === "dashboard" && (
            <div className="space-y-6">
              <div className="grid grid-cols-2 lg:grid-cols-4 gap-4">
                {stats.map((stat) => (
                  <div key={stat.label} className="bg-white rounded-xl p-5 shadow-sm border border-gray-100">
                    <div className="flex items-center justify-between mb-3">
                      <div className={`w-10 h-10 ${stat.bg} rounded-xl flex items-center justify-center`}>
                        <stat.icon size={20} className={stat.color} />
                      </div>
                      <span className="text-green-600 text-xs font-bold bg-green-50 px-2 py-0.5 rounded-full">
                        {stat.change}
                      </span>
                    </div>
                    <div className="font-black text-gray-800 text-xl">{stat.value}</div>
                    <div className="text-gray-500 text-xs font-semibold mt-0.5">{stat.label}</div>
                  </div>
                ))}
              </div>

              <div className="bg-white rounded-xl shadow-sm border border-gray-100">
                <div className="flex items-center justify-between p-5 border-b">
                  <h3 className="font-black text-gray-800">Produtos Recentes</h3>
                  <button
                    onClick={() => setSection("products")}
                    className="text-orange-500 text-sm font-bold hover:text-orange-600"
                  >
                    Ver todos
                  </button>
                </div>
                <div className="overflow-x-auto">
                  <table className="w-full text-sm">
                    <thead className="bg-gray-50">
                      <tr>
                        <th className="text-left p-4 font-bold text-gray-500 text-xs uppercase">Produto</th>
                        <th className="text-left p-4 font-bold text-gray-500 text-xs uppercase">Preço</th>
                        <th className="text-left p-4 font-bold text-gray-500 text-xs uppercase">Estoque</th>
                        <th className="text-left p-4 font-bold text-gray-500 text-xs uppercase">Vendidos</th>
                      </tr>
                    </thead>
                    <tbody>
                      {PRODUCTS.slice(0, 5).map((product) => (
                        <tr key={product.id} className="border-t border-gray-100 hover:bg-gray-50">
                          <td className="p-4">
                            <div className="flex items-center gap-3">
                              <img src={product.image} alt={product.name} className="w-10 h-10 rounded-lg object-cover" />
                              <span className="font-semibold text-gray-800 line-clamp-1">{product.name}</span>
                            </div>
                          </td>
                          <td className="p-4 font-bold text-orange-600">{formatPrice(product.price)}</td>
                          <td className="p-4">
                            <span className={`text-xs font-bold px-2 py-1 rounded-full ${
                              product.stock > 50 ? "bg-green-100 text-green-700" :
                              product.stock > 20 ? "bg-yellow-100 text-yellow-700" :
                              "bg-red-100 text-red-700"
                            }`}>
                              {product.stock}
                            </span>
                          </td>
                          <td className="p-4 text-gray-600 font-semibold">{product.sold}</td>
                        </tr>
                      ))}
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
          )}

          {/* ===== PRODUCTS ===== */}
          {section === "products" && (
            <div className="space-y-4">
              <div className="flex items-center justify-between">
                <p className="text-gray-500 text-sm font-semibold">{PRODUCTS.length} produtos cadastrados</p>
                <button
                  onClick={() => {
                    setEditingProduct({
                      name: "",
                      description: "",
                      price: 0,
                      category: "vestidos",
                      image: "",
                      colors: [],
                      sizes: [],
                      stock: 0,
                    });
                    setShowProductForm(true);
                  }}
                  className="flex items-center gap-2 bg-orange-500 hover:bg-orange-600 text-white font-bold px-4 py-2 rounded-xl text-sm transition-colors"
                >
                  <Plus size={16} />
                  Novo Produto
                </button>
              </div>

              {/* Product Form Modal */}
              {showProductForm && editingProduct && (
                <div className="fixed inset-0 bg-black/50 flex items-center justify-center z-50 p-4">
                  <div className="bg-white rounded-xl max-w-2xl w-full max-h-[90vh] overflow-y-auto">
                    <div className="flex items-center justify-between p-6 border-b sticky top-0 bg-white">
                      <h3 className="font-black text-gray-800 text-lg">Novo Produto</h3>
                      <button onClick={() => setShowProductForm(false)} className="text-gray-400 hover:text-gray-600">
                        <X size={24} />
                      </button>
                    </div>

                    <div className="p-6 space-y-4">
                      <div>
                        <label className="block text-sm font-bold text-gray-700 mb-1">Nome do Produto</label>
                        <input
                          type="text"
                          value={editingProduct.name}
                          onChange={(e) => setEditingProduct({ ...editingProduct, name: e.target.value })}
                          placeholder="Ex: Camiseta Premium"
                          className="w-full border border-gray-200 rounded-lg px-3 py-2 text-sm outline-none focus:border-orange-400"
                        />
                      </div>

                      <div>
                        <label className="block text-sm font-bold text-gray-700 mb-1">Descrição</label>
                        <textarea
                          value={editingProduct.description}
                          onChange={(e) => setEditingProduct({ ...editingProduct, description: e.target.value })}
                          placeholder="Descreva o produto..."
                          className="w-full border border-gray-200 rounded-lg px-3 py-2 text-sm outline-none focus:border-orange-400 h-20"
                        />
                      </div>

                      <div className="grid grid-cols-2 gap-3">
                        <div>
                          <label className="block text-sm font-bold text-gray-700 mb-1">Preço (R$)</label>
                          <input
                            type="number"
                            value={editingProduct.price}
                            onChange={(e) => setEditingProduct({ ...editingProduct, price: parseFloat(e.target.value) })}
                            placeholder="89.90"
                            className="w-full border border-gray-200 rounded-lg px-3 py-2 text-sm outline-none focus:border-orange-400"
                          />
                        </div>
                        <div>
                          <label className="block text-sm font-bold text-gray-700 mb-1">Preço Original (opcional)</label>
                          <input
                            type="number"
                            value={editingProduct.originalPrice || ""}
                            onChange={(e) => setEditingProduct({ ...editingProduct, originalPrice: parseFloat(e.target.value) })}
                            placeholder="129.90"
                            className="w-full border border-gray-200 rounded-lg px-3 py-2 text-sm outline-none focus:border-orange-400"
                          />
                        </div>
                      </div>

                      <div className="grid grid-cols-2 gap-3">
                        <div>
                          <label className="block text-sm font-bold text-gray-700 mb-1">Categoria</label>
                          <select
                            value={editingProduct.category}
                            onChange={(e) => setEditingProduct({ ...editingProduct, category: e.target.value })}
                            className="w-full border border-gray-200 rounded-lg px-3 py-2 text-sm outline-none focus:border-orange-400"
                          >
                            <option value="vestidos">Vestidos</option>
                            <option value="blusas">Blusas</option>
                            <option value="calcas">Calças</option>
                            <option value="saias">Saias</option>
                            <option value="shorts">Shorts</option>
                            <option value="conjuntos">Conjuntos</option>
                            <option value="acessorios">Acessórios</option>
                            <option value="calcados">Calçados</option>
                          </select>
                        </div>
                        <div>
                          <label className="block text-sm font-bold text-gray-700 mb-1">Estoque</label>
                          <input
                            type="number"
                            value={editingProduct.stock}
                            onChange={(e) => setEditingProduct({ ...editingProduct, stock: parseInt(e.target.value) })}
                            placeholder="50"
                            className="w-full border border-gray-200 rounded-lg px-3 py-2 text-sm outline-none focus:border-orange-400"
                          />
                        </div>
                      </div>

                      <div>
                        <label className="block text-sm font-bold text-gray-700 mb-1">URL da Imagem</label>
                        <input
                          type="text"
                          value={editingProduct.image}
                          onChange={(e) => setEditingProduct({ ...editingProduct, image: e.target.value })}
                          placeholder="https://..."
                          className="w-full border border-gray-200 rounded-lg px-3 py-2 text-sm outline-none focus:border-orange-400"
                        />
                        {editingProduct.image && (
                          <img src={editingProduct.image} alt="preview" className="w-20 h-20 rounded-lg mt-2 object-cover" />
                        )}
                      </div>

                      <div>
                        <label className="block text-sm font-bold text-gray-700 mb-2">Cores</label>
                        <div className="flex gap-2 mb-2">
                          {editingProduct.colors.map((color) => (
                            <div key={color} className="flex items-center gap-1 bg-gray-100 px-2 py-1 rounded-lg text-sm">
                              <span>{color}</span>
                              <button
                                onClick={() => setEditingProduct({
                                  ...editingProduct,
                                  colors: editingProduct.colors.filter(c => c !== color)
                                })}
                                className="text-red-500 hover:text-red-700"
                              >
                                <X size={14} />
                              </button>
                            </div>
                          ))}
                        </div>
                        <div className="flex gap-2">
                          <input
                            type="text"
                            value={newColor}
                            onChange={(e) => setNewColor(e.target.value)}
                            placeholder="Ex: Laranja"
                            className="flex-1 border border-gray-200 rounded-lg px-3 py-2 text-sm outline-none focus:border-orange-400"
                          />
                          <button
                            onClick={() => {
                              if (newColor && !editingProduct.colors.includes(newColor)) {
                                setEditingProduct({
                                  ...editingProduct,
                                  colors: [...editingProduct.colors, newColor]
                                });
                                setNewColor("");
                              }
                            }}
                            className="bg-orange-500 text-white px-3 py-2 rounded-lg text-sm font-bold hover:bg-orange-600"
                          >
                            Adicionar
                          </button>
                        </div>
                      </div>

                      <div>
                        <label className="block text-sm font-bold text-gray-700 mb-2">Tamanhos</label>
                        <div className="flex gap-2 mb-2">
                          {editingProduct.sizes.map((size) => (
                            <div key={size} className="flex items-center gap-1 bg-gray-100 px-2 py-1 rounded-lg text-sm">
                              <span>{size}</span>
                              <button
                                onClick={() => setEditingProduct({
                                  ...editingProduct,
                                  sizes: editingProduct.sizes.filter(s => s !== size)
                                })}
                                className="text-red-500 hover:text-red-700"
                              >
                                <X size={14} />
                              </button>
                            </div>
                          ))}
                        </div>
                        <div className="flex gap-2">
                          <input
                            type="text"
                            value={newSize}
                            onChange={(e) => setNewSize(e.target.value)}
                            placeholder="Ex: P, M, G, GG"
                            className="flex-1 border border-gray-200 rounded-lg px-3 py-2 text-sm outline-none focus:border-orange-400"
                          />
                          <button
                            onClick={() => {
                              if (newSize && !editingProduct.sizes.includes(newSize)) {
                                setEditingProduct({
                                  ...editingProduct,
                                  sizes: [...editingProduct.sizes, newSize]
                                });
                                setNewSize("");
                              }
                            }}
                            className="bg-orange-500 text-white px-3 py-2 rounded-lg text-sm font-bold hover:bg-orange-600"
                          >
                            Adicionar
                          </button>
                        </div>
                      </div>

                      <div className="flex gap-3 pt-4 border-t">
                        <button
                          onClick={() => setShowProductForm(false)}
                          className="flex-1 border border-gray-200 text-gray-700 font-bold px-4 py-2 rounded-lg hover:bg-gray-50"
                        >
                          Cancelar
                        </button>
                        <button
                          onClick={handleSaveProduct}
                          className="flex-1 bg-orange-500 hover:bg-orange-600 text-white font-bold px-4 py-2 rounded-lg flex items-center justify-center gap-2"
                        >
                          <Save size={16} />
                          Salvar Produto
                        </button>
                      </div>
                    </div>
                  </div>
                </div>
              )}

              {/* Products Table */}
              <div className="bg-white rounded-xl shadow-sm border border-gray-100 overflow-hidden">
                <div className="overflow-x-auto">
                  <table className="w-full text-sm">
                    <thead className="bg-gray-50">
                      <tr>
                        <th className="text-left p-4 font-bold text-gray-500 text-xs uppercase">Produto</th>
                        <th className="text-left p-4 font-bold text-gray-500 text-xs uppercase">Preço</th>
                        <th className="text-left p-4 font-bold text-gray-500 text-xs uppercase">Cores</th>
                        <th className="text-left p-4 font-bold text-gray-500 text-xs uppercase">Tamanhos</th>
                        <th className="text-left p-4 font-bold text-gray-500 text-xs uppercase">Estoque</th>
                        <th className="text-left p-4 font-bold text-gray-500 text-xs uppercase">Ações</th>
                      </tr>
                    </thead>
                    <tbody>
                      {PRODUCTS.filter(p =>
                        !searchQuery || p.name.toLowerCase().includes(searchQuery.toLowerCase())
                      ).map((product) => (
                        <tr key={product.id} className="border-t border-gray-100 hover:bg-gray-50">
                          <td className="p-4">
                            <div className="flex items-center gap-3">
                              <img src={product.image} alt={product.name} className="w-10 h-10 rounded-lg object-cover" />
                              <span className="font-semibold text-gray-800 line-clamp-1">{product.name}</span>
                            </div>
                          </td>
                          <td className="p-4 font-bold text-orange-600">{formatPrice(product.price)}</td>
                          <td className="p-4 text-xs text-gray-600">—</td>
                          <td className="p-4 text-xs text-gray-600">—</td>
                          <td className="p-4">
                            <span className={`text-xs font-bold px-2 py-1 rounded-full ${
                              product.stock > 50 ? "bg-green-100 text-green-700" :
                              product.stock > 20 ? "bg-yellow-100 text-yellow-700" :
                              "bg-red-100 text-red-700"
                            }`}>
                              {product.stock}
                            </span>
                          </td>
                          <td className="p-4">
                            <div className="flex gap-1">
                              <button onClick={() => toast.info("Editor em breve!")} className="w-7 h-7 bg-blue-50 text-blue-600 rounded-lg flex items-center justify-center hover:bg-blue-100">
                                <Edit2 size={13} />
                              </button>
                              <button onClick={() => toast.success("Produto removido!")} className="w-7 h-7 bg-red-50 text-red-500 rounded-lg flex items-center justify-center hover:bg-red-100">
                                <Trash2 size={13} />
                              </button>
                            </div>
                          </td>
                        </tr>
                      ))}
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
          )}

          {/* ===== BANNERS ===== */}
          {section === "banners" && (
            <div className="space-y-4">
              <div className="flex items-center justify-between">
                <p className="text-gray-500 text-sm font-semibold">{BANNERS.length} banners ativos</p>
                <button
                  onClick={() => {
                    setEditingBanner({ title: "", subtitle: "", image: "", link: "" });
                    setShowBannerForm(true);
                  }}
                  className="flex items-center gap-2 bg-orange-500 hover:bg-orange-600 text-white font-bold px-4 py-2 rounded-xl text-sm transition-colors"
                >
                  <Plus size={16} />
                  Novo Banner
                </button>
              </div>

              {showBannerForm && editingBanner && (
                <div className="bg-white rounded-xl shadow-sm border border-gray-100 p-6">
                  <h3 className="font-black text-gray-800 text-lg mb-4">Novo Banner</h3>
                  <div className="space-y-4">
                    <input
                      type="text"
                      value={editingBanner.title}
                      onChange={(e) => setEditingBanner({ ...editingBanner, title: e.target.value })}
                      placeholder="Título"
                      className="w-full border border-gray-200 rounded-lg px-3 py-2 text-sm outline-none focus:border-orange-400"
                    />
                    <input
                      type="text"
                      value={editingBanner.subtitle}
                      onChange={(e) => setEditingBanner({ ...editingBanner, subtitle: e.target.value })}
                      placeholder="Subtítulo"
                      className="w-full border border-gray-200 rounded-lg px-3 py-2 text-sm outline-none focus:border-orange-400"
                    />
                    <input
                      type="text"
                      value={editingBanner.image}
                      onChange={(e) => setEditingBanner({ ...editingBanner, image: e.target.value })}
                      placeholder="URL da imagem"
                      className="w-full border border-gray-200 rounded-lg px-3 py-2 text-sm outline-none focus:border-orange-400"
                    />
                    <input
                      type="text"
                      value={editingBanner.link}
                      onChange={(e) => setEditingBanner({ ...editingBanner, link: e.target.value })}
                      placeholder="Link (ex: /produtos)"
                      className="w-full border border-gray-200 rounded-lg px-3 py-2 text-sm outline-none focus:border-orange-400"
                    />
                    <div className="flex gap-3">
                      <button
                        onClick={() => setShowBannerForm(false)}
                        className="flex-1 border border-gray-200 text-gray-700 font-bold px-4 py-2 rounded-lg"
                      >
                        Cancelar
                      </button>
                      <button
                        onClick={handleSaveBanner}
                        className="flex-1 bg-orange-500 hover:bg-orange-600 text-white font-bold px-4 py-2 rounded-lg"
                      >
                        Salvar
                      </button>
                    </div>
                  </div>
                </div>
              )}

              <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
                {BANNERS.map((banner) => (
                  <div key={banner.id} className="bg-white rounded-xl shadow-sm border border-gray-100 overflow-hidden">
                    <div className="relative">
                      <img src={banner.image} alt={banner.title} className="w-full h-36 object-cover" />
                      <div className="absolute top-2 right-2 flex gap-1">
                        <button onClick={() => toast.info("Editor em breve!")} className="w-7 h-7 bg-white/90 text-blue-600 rounded-lg flex items-center justify-center shadow-sm">
                          <Edit2 size={13} />
                        </button>
                        <button onClick={() => toast.success("Banner removido!")} className="w-7 h-7 bg-white/90 text-red-500 rounded-lg flex items-center justify-center shadow-sm">
                          <Trash2 size={13} />
                        </button>
                      </div>
                    </div>
                    <div className="p-3">
                      <p className="font-bold text-gray-800 text-sm">{banner.title}</p>
                      <p className="text-gray-500 text-xs mt-0.5">{banner.subtitle}</p>
                    </div>
                  </div>
                ))}
              </div>
            </div>
          )}

          {/* ===== CAMPAIGNS ===== */}
          {section === "campaigns" && (
            <div className="space-y-4">
              <div className="flex items-center justify-between">
                <p className="text-gray-500 text-sm font-semibold">{CAMPAIGNS.length} campanhas ativas</p>
                <button
                  onClick={() => {
                    setEditingCampaign({
                      title: "",
                      subtitle: "",
                      image: "",
                      discount: 0,
                      productId: 1,
                      startDate: "",
                      endDate: ""
                    });
                    setShowCampaignForm(true);
                  }}
                  className="flex items-center gap-2 bg-orange-500 hover:bg-orange-600 text-white font-bold px-4 py-2 rounded-xl text-sm transition-colors"
                >
                  <Plus size={16} />
                  Nova Campanha
                </button>
              </div>

              {showCampaignForm && editingCampaign && (
                <div className="bg-white rounded-xl shadow-sm border border-gray-100 p-6">
                  <h3 className="font-black text-gray-800 text-lg mb-4">Nova Campanha</h3>
                  <div className="space-y-4">
                    <input
                      type="text"
                      value={editingCampaign.title}
                      onChange={(e) => setEditingCampaign({ ...editingCampaign, title: e.target.value })}
                      placeholder="Título da campanha"
                      className="w-full border border-gray-200 rounded-lg px-3 py-2 text-sm outline-none focus:border-orange-400"
                    />
                    <input
                      type="text"
                      value={editingCampaign.subtitle}
                      onChange={(e) => setEditingCampaign({ ...editingCampaign, subtitle: e.target.value })}
                      placeholder="Subtítulo"
                      className="w-full border border-gray-200 rounded-lg px-3 py-2 text-sm outline-none focus:border-orange-400"
                    />
                    <div className="grid grid-cols-2 gap-3">
                      <input
                        type="number"
                        value={editingCampaign.discount}
                        onChange={(e) => setEditingCampaign({ ...editingCampaign, discount: parseInt(e.target.value) })}
                        placeholder="Desconto (%)"
                        className="w-full border border-gray-200 rounded-lg px-3 py-2 text-sm outline-none focus:border-orange-400"
                      />
                      <select
                        value={editingCampaign.productId}
                        onChange={(e) => setEditingCampaign({ ...editingCampaign, productId: parseInt(e.target.value) })}
                        className="w-full border border-gray-200 rounded-lg px-3 py-2 text-sm outline-none focus:border-orange-400"
                      >
                        {PRODUCTS.map(p => (
                          <option key={p.id} value={p.id}>{p.name}</option>
                        ))}
                      </select>
                    </div>
                    <input
                      type="text"
                      value={editingCampaign.image}
                      onChange={(e) => setEditingCampaign({ ...editingCampaign, image: e.target.value })}
                      placeholder="URL da imagem"
                      className="w-full border border-gray-200 rounded-lg px-3 py-2 text-sm outline-none focus:border-orange-400"
                    />
                    <div className="grid grid-cols-2 gap-3">
                      <input
                        type="date"
                        value={editingCampaign.startDate}
                        onChange={(e) => setEditingCampaign({ ...editingCampaign, startDate: e.target.value })}
                        className="w-full border border-gray-200 rounded-lg px-3 py-2 text-sm outline-none focus:border-orange-400"
                      />
                      <input
                        type="date"
                        value={editingCampaign.endDate}
                        onChange={(e) => setEditingCampaign({ ...editingCampaign, endDate: e.target.value })}
                        className="w-full border border-gray-200 rounded-lg px-3 py-2 text-sm outline-none focus:border-orange-400"
                      />
                    </div>
                    <div className="flex gap-3">
                      <button
                        onClick={() => setShowCampaignForm(false)}
                        className="flex-1 border border-gray-200 text-gray-700 font-bold px-4 py-2 rounded-lg"
                      >
                        Cancelar
                      </button>
                      <button
                        onClick={handleSaveCampaign}
                        className="flex-1 bg-orange-500 hover:bg-orange-600 text-white font-bold px-4 py-2 rounded-lg"
                      >
                        Salvar
                      </button>
                    </div>
                  </div>
                </div>
              )}

              <div className="grid grid-cols-1 sm:grid-cols-2 gap-4">
                {CAMPAIGNS.map((campaign) => (
                  <div key={campaign.id} className="bg-white rounded-xl shadow-sm border border-gray-100 overflow-hidden flex">
                    <img src={campaign.image} alt={campaign.title} className="w-28 h-28 object-cover flex-shrink-0" />
                    <div className="p-4 flex-1">
                      <span className="text-xs font-black text-white px-2 py-0.5 rounded" style={{ backgroundColor: campaign.color }}>
                        {campaign.badge}
                      </span>
                      <h3 className="font-black text-gray-800 text-sm mt-1">{campaign.title}</h3>
                      <p className="text-gray-500 text-xs">{campaign.subtitle}</p>
                      <div className="flex gap-1 mt-2">
                        <button onClick={() => toast.info("Editor em breve!")} className="w-7 h-7 bg-blue-50 text-blue-600 rounded-lg flex items-center justify-center">
                          <Edit2 size={13} />
                        </button>
                        <button onClick={() => toast.success("Campanha removida!")} className="w-7 h-7 bg-red-50 text-red-500 rounded-lg flex items-center justify-center">
                          <Trash2 size={13} />
                        </button>
                      </div>
                    </div>
                  </div>
                ))}
              </div>
            </div>
          )}

          {/* ===== PAYMENTS ===== */}
          {section === "payments" && (
            <div className="space-y-6">
              <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
                {/* PIX Configuration */}
                <div className="bg-white rounded-xl shadow-sm border border-gray-100 p-6">
                  <div className="flex items-center gap-3 mb-4">
                    <div className="w-10 h-10 bg-blue-100 rounded-lg flex items-center justify-center">
                      <DollarSign size={20} className="text-blue-600" />
                    </div>
                    <h3 className="font-black text-gray-800 text-lg">PIX</h3>
                  </div>
                  <div className="space-y-4">
                    <div>
                      <label className="block text-sm font-bold text-gray-700 mb-1">Tipo de Chave</label>
                      <select
                        value={pixKeyType}
                        onChange={(e) => setPixKeyType(e.target.value as any)}
                        className="w-full border border-gray-200 rounded-lg px-3 py-2 text-sm outline-none focus:border-orange-400"
                      >
                        <option value="cpf">CPF</option>
                        <option value="email">Email</option>
                        <option value="cnpj">CNPJ</option>
                      </select>
                    </div>
                    <div>
                      <label className="block text-sm font-bold text-gray-700 mb-1">Chave PIX</label>
                      <input
                        type="text"
                        value={pixKey}
                        onChange={(e) => setPixKey(e.target.value)}
                        placeholder={pixKeyType === "cpf" ? "123.456.789-00" : pixKeyType === "email" ? "seu@email.com" : "12.345.678/0001-00"}
                        className="w-full border border-gray-200 rounded-lg px-3 py-2 text-sm outline-none focus:border-orange-400"
                      />
                    </div>
                    <div className="bg-blue-50 border border-blue-200 rounded-lg p-3">
                      <p className="text-xs text-blue-700">
                        <strong>Dica:</strong> Registre sua chave PIX no app do seu banco antes de usar aqui.
                      </p>
                    </div>
                  </div>
                </div>

                {/* Mercado Pago Configuration */}
                <div className="bg-white rounded-xl shadow-sm border border-gray-100 p-6">
                  <div className="flex items-center gap-3 mb-4">
                    <div className="w-10 h-10 bg-yellow-100 rounded-lg flex items-center justify-center">
                      <DollarSign size={20} className="text-yellow-600" />
                    </div>
                    <h3 className="font-black text-gray-800 text-lg">Mercado Pago</h3>
                  </div>
                  <div className="space-y-4">
                    <div>
                      <label className="block text-sm font-bold text-gray-700 mb-1">Access Token</label>
                      <input
                        type="password"
                        value={mercadoPagoToken}
                        onChange={(e) => setMercadoPagoToken(e.target.value)}
                        placeholder="APP_USR_..."
                        className="w-full border border-gray-200 rounded-lg px-3 py-2 text-sm outline-none focus:border-orange-400"
                      />
                    </div>
                    <div>
                      <label className="block text-sm font-bold text-gray-700 mb-1">Public Key</label>
                      <input
                        type="password"
                        value={mercadoPagoPublicKey}
                        onChange={(e) => setMercadoPagoPublicKey(e.target.value)}
                        placeholder="APP_USR_..."
                        className="w-full border border-gray-200 rounded-lg px-3 py-2 text-sm outline-none focus:border-orange-400"
                      />
                    </div>
                    <div className="bg-yellow-50 border border-yellow-200 rounded-lg p-3">
                      <p className="text-xs text-yellow-700">
                        <strong>Dica:</strong> Obtenha suas chaves em https://www.mercadopago.com.br/developers
                      </p>
                    </div>
                  </div>
                </div>
              </div>

              <button
                onClick={handleSavePayments}
                className="w-full bg-orange-500 hover:bg-orange-600 text-white font-bold px-4 py-3 rounded-xl flex items-center justify-center gap-2"
              >
                <Save size={18} />
                Salvar Configurações de Pagamento
              </button>
            </div>
          )}

          {/* ===== ORDERS ===== */}
          {section === "orders" && (
            <div className="bg-white rounded-xl shadow-sm border border-gray-100 overflow-hidden">
              <div className="p-6 border-b">
                <h3 className="font-black text-gray-800 text-lg">Pedidos Recentes</h3>
              </div>
              <div className="overflow-x-auto">
                <table className="w-full text-sm">
                  <thead className="bg-gray-50">
                    <tr>
                      <th className="text-left p-4 font-bold text-gray-500 text-xs uppercase">Pedido</th>
                      <th className="text-left p-4 font-bold text-gray-500 text-xs uppercase">Cliente</th>
                      <th className="text-left p-4 font-bold text-gray-500 text-xs uppercase">Valor</th>
                      <th className="text-left p-4 font-bold text-gray-500 text-xs uppercase">Status</th>
                      <th className="text-left p-4 font-bold text-gray-500 text-xs uppercase">Data</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr className="border-t border-gray-100 hover:bg-gray-50">
                      <td className="p-4 font-bold text-gray-800">#12345</td>
                      <td className="p-4 text-gray-600">João Silva</td>
                      <td className="p-4 font-bold text-orange-600">R$ 299,90</td>
                      <td className="p-4">
                        <span className="text-xs font-bold bg-green-100 text-green-700 px-2 py-1 rounded-full">Pago</span>
                      </td>
                      <td className="p-4 text-gray-500">16/04/2026</td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>
          )}

          {/* ===== TRANSACTIONS ===== */}
          {section === "transactions" && (
            <div className="bg-white rounded-xl shadow-sm border border-gray-100 overflow-hidden">
              <div className="p-6 border-b">
                <h3 className="font-black text-gray-800 text-lg">Histórico de Transações</h3>
              </div>
              <div className="overflow-x-auto">
                <table className="w-full text-sm">
                  <thead className="bg-gray-50">
                    <tr>
                      <th className="text-left p-4 font-bold text-gray-500 text-xs uppercase">Transação</th>
                      <th className="text-left p-4 font-bold text-gray-500 text-xs uppercase">Método</th>
                      <th className="text-left p-4 font-bold text-gray-500 text-xs uppercase">Valor</th>
                      <th className="text-left p-4 font-bold text-gray-500 text-xs uppercase">Status</th>
                      <th className="text-left p-4 font-bold text-gray-500 text-xs uppercase">Data</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr className="border-t border-gray-100 hover:bg-gray-50">
                      <td className="p-4 font-bold text-gray-800">#TRX001</td>
                      <td className="p-4 text-gray-600">PIX</td>
                      <td className="p-4 font-bold text-green-600">R$ 299,90</td>
                      <td className="p-4">
                        <span className="text-xs font-bold bg-green-100 text-green-700 px-2 py-1 rounded-full">Aprovado</span>
                      </td>
                      <td className="p-4 text-gray-500">16/04/2026</td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>
          )}
        </main>
      </div>
    </div>
  );
}

